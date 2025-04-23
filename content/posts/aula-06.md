+++
title = 'Aula 06 - Desenvolvimento de Patch para o Kernel Linux'
date = 2025-04-09T14:00:00-03:00
draft = false
tags = ['aula', 'kernel linux', 'patch', 'refatoração', 'contribuição']
+++

# Aula 06 - Desenvolvimento de Patch para o Kernel Linux

**Data:** 09/04/2025

## O que aprendi hoje

Na aula de hoje, o professor apresentou sugestões de patches para trabalharmos em equipe e contribuir diretamente com o kernel Linux. Os principais tópicos abordados foram:

1. **Processo de contribuição para o kernel Linux**
   - Ciclo de desenvolvimento e revisão de código no kernel
   - Processo de submissão de patches
   - Diretrizes e boas práticas para contribuições

2. **Identificação de problemas no código**
   - Análise de código duplicado e oportunidades de refatoração
   - Técnicas para aumentar a legibilidade e manutenibilidade do código
   - Avaliação de impacto das mudanças propostas

3. **Refatoração de código**
   - Técnicas de refatoração segura para sistemas críticos
   - Extração de métodos e parâmetros
   - Preservação do comportamento original do código

4. **Qualidade de código no kernel**
   - Padrões de codificação específicos do kernel Linux
   - Importância da reutilização de código
   - Equilíbrio entre otimização e legibilidade

## Atividades realizadas

O professor apresentou várias oportunidades de patches para o kernel Linux, e eu e minha dupla escolhemos trabalhar em um problema específico de duplicação de código:

- Identificamos código duplicado no driver de proximidade SX9500, especificamente nas funções `sx9500_buffer_postenable` e `sx9500_buffer_predisable`
- Analisamos o código duplicado e verificamos que ambas as funções seguiam o mesmo algoritmo, mas realizavam operações opostas sobre os canais (incremento vs. decremento de usuários)
- Desenvolvemos uma solução que extraía o algoritmo comum para uma função auxiliar parametrizada

Nossa solução consistiu em:
1. Criar uma nova função `sx9500_buffer_manage_chan_users` que encapsula a lógica comum
2. Adicionar um parâmetro booleano para controlar se a operação é de incremento ou decremento
3. Adaptar as funções originais para chamar a nova função com o parâmetro apropriado
4. Garantir que o comportamento original fosse preservado, incluindo o tratamento de erros e rollback

O código refatorado resultou em uma redução significativa de linhas de código duplicado, mantendo a mesma funcionalidade e tornando o código mais fácil de manter.

### Código Original vs. Refatorado

**Código Original:**
```c
static int sx9500_buffer_postenable(struct iio_dev *indio_dev)
{
	struct sx9500_data *data = iio_priv(indio_dev);
	int ret = 0, i;

	mutex_lock(&data->mutex);

	for (i = 0; i < SX9500_NUM_CHANNELS; i++)
		if (test_bit(i, indio_dev->active_scan_mask)) {
			ret = sx9500_inc_chan_users(data, i);
			if (ret)
				break;
		}

	if (ret)
		for (i = i - 1; i >= 0; i--)
			if (test_bit(i, indio_dev->active_scan_mask))
				sx9500_dec_chan_users(data, i);

	mutex_unlock(&data->mutex);

	return ret;
}

static int sx9500_buffer_predisable(struct iio_dev *indio_dev)
{
	struct sx9500_data *data = iio_priv(indio_dev);
	int ret = 0, i;

	mutex_lock(&data->mutex);

	for (i = 0; i < SX9500_NUM_CHANNELS; i++)
		if (test_bit(i, indio_dev->active_scan_mask)) {
			ret = sx9500_dec_chan_users(data, i);
			if (ret)
				break;
		}

	if (ret)
		for (i = i - 1; i >= 0; i--)
			if (test_bit(i, indio_dev->active_scan_mask))
				sx9500_inc_chan_users(data, i);

	mutex_unlock(&data->mutex);

	return ret;
}
```

**Código Refatorado:**
```c
static int sx9500_buffer_manage_chan_users(struct iio_dev *indio_dev, bool enable)
{
	struct sx9500_data *data = iio_priv(indio_dev);
	int ret = 0, i;

	mutex_lock(&data->mutex);

	for (i = 0; i < SX9500_NUM_CHANNELS; i++) {
		if (test_bit(i, indio_dev->active_scan_mask)) {
			if (enable)
				ret = sx9500_inc_chan_users(data, i);
			else
				ret = sx9500_dec_chan_users(data, i);
			if (ret)
				break;
		}
	}

	if (ret) {
		for (i = i - 1; i >= 0; i--) {
			if (test_bit(i, indio_dev->active_scan_mask)) {
				if (enable)
					sx9500_dec_chan_users(data, i);
				else
					sx9500_inc_chan_users(data, i);
			}
		}
	}

	mutex_unlock(&data->mutex);

	return ret;
}

static int sx9500_buffer_postenable(struct iio_dev *indio_dev)
{
	return sx9500_buffer_manage_chan_users(indio_dev, true);
}

static int sx9500_buffer_predisable(struct iio_dev *indio_dev)
{
	return sx9500_buffer_manage_chan_users(indio_dev, false);
}
```

Como pode ser observado, nossa refatoração permitiu reduzir a duplicação de código, mantendo a mesma funcionalidade. A nova função `sx9500_buffer_manage_chan_users` encapsula o algoritmo comum, enquanto o parâmetro `enable` controla se a operação é de incremento ou decremento. As funções originais foram reduzidas a simples chamadas para a nova função, melhorando significativamente a legibilidade e manutenibilidade do código.

## Reflexões

Esta aula representou um passo importante na disciplina, pois passamos de apenas estudar o código do kernel para efetivamente contribuir com ele. A experiência de identificar código duplicado e propor uma solução foi muito valiosa, pois exigiu um entendimento profundo não apenas do código em si, mas também dos princípios e padrões de desenvolvimento do kernel Linux.

Aprendi que, mesmo em um sistema crítico e complexo como o kernel, há oportunidades para melhorias simples que podem ter um impacto positivo na manutenibilidade do código. A refatoração que realizamos, embora relativamente pequena, demonstra como é possível reduzir a redundância e aumentar a clareza do código sem alterar seu comportamento.
