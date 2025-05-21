+++
title = 'Aula 09 - Primeira Contribuição para o GNOME Clocks'
date = 2025-05-07T14:00:00-03:00
draft = false
tags = ['aula', 'gnome', 'contribuição', 'vala', 'gnome-clocks']
+++

# Aula 09 - Primeira Contribuição para o GNOME Clocks

**Data:** 07/05/2025

## O que aprendi hoje

Na aula de hoje, comecei minha primeira contribuição para o projeto GNOME Clocks, trabalhando na issue [#359](https://gitlab.gnome.org/GNOME/gnome-clocks/-/issues/359). Também tentei trabalhar na issue [#329](https://gitlab.gnome.org/GNOME/gnome-clocks/-/issues/329), que trata de um problema relacionado à detecção de localizações duplicadas ao adicionar cidades no aplicativo.

### Sobre a issue #329

A issue relata que, ao adicionar cidades no GNOME Clocks, é possível adicionar a mesma cidade mais de uma vez, pois o sistema não detecta corretamente duplicatas. Investiguei o código responsável por verificar se uma localização já existe e percebi que a função `location_exists` não estava funcionando como esperado. Descobri que o problema pode estar relacionado à biblioteca [libgweather](https://gitlab.gnome.org/GNOME/libgweather), utilizada para manipular as localizações.

Durante a investigação, notei que o código da localização retornada por `GWeather.Location.get_world()` estava vazio, o que fazia com que a comparação de igualdade falhasse:

```vala
public bool location_exists (GWeather.Location location) {
    var exists = false;
    var n = locations.get_n_items ();
    print("location code: " + location.get_code());
    for (int i = 0; i < n; i++) {
        var l = (Item) locations.get_object (i);
        if (l.location.equal (location)) {
            exists = true;
            break;
        }
    }
    return exists;
}
```

O output mostrava que o código da localização era vazio (`location code: `), mas ao imprimir o código da variável `l`, o valor era preenchido corretamente. Isso me levou a suspeitar de um possível bug na biblioteca gweather, especificamente na função de comparação de localizações ([código da função equals](https://gitlab.gnome.org/GNOME/libgweather/-/blob/main/libgweather/gweather-location.c)).

Comentei minhas descobertas na issue, mas até o momento não obtive resposta:

> I did some research and I think the problem is related to the type of the location returned by the 
> `var world_location = GWeather.Location.get_world ();`
> I checked the equals function in the gweather lib: https://gitlab.gnome.org/GNOME/libgweather/-/blob/main/libgweather/gweather-location.c
> 
> It seems to checks the location code in the line:
> ```c
>    if (g_strcmp0 (gweather_location_get_code (one),
>                    gweather_location_get_code (two)) != 0)
>         return FALSE;
> ```
> Printing the code before the comparison showed that it was empty:
> ```
> public bool location_exists (GWeather.Location location) {
>         var exists = false;
>         var n = locations.get_n_items ();
>         print("location code: " + location.get_code());
>         for (int i = 0; i < n; i++) {
>             var l = (Item) locations.get_object (i);
>             if (l.location.equal (location)) {
>                 exists = true;
>                 break;
>             }
>         }
>         return exists;
>     }
> ```
> Output: `location code: `
> But printing the code of variable `l` worked.

## Sobre a issue #359

A issue [#359](https://gitlab.gnome.org/GNOME/gnome-clocks/-/issues/359) relata que, ao deletar um alarme no GNOME Clocks, não há uma notificação (toast) informando que o alarme foi removido, nem a possibilidade de desfazer a ação. Isso pode ser problemático caso o usuário remova um alarme por engano, pois não há como recuperá-lo facilmente.

Para resolver esse problema, implementei uma funcionalidade de toast com opção de desfazer a deleção do alarme. Agora, ao deletar um alarme, aparece um toast com a mensagem "Alarm deleted" e um botão "Undo". Caso o usuário clique em "Undo", o alarme é restaurado imediatamente.

## Código Implementado

```vala
private Adw.Toast? delete_toast;
private Alarm.Item? deleted_alarm;

// ... existing code ...

row.remove_alarm.connect (() => {
    deleted_alarm = (Item) item;
    alarms.delete_item ((Item) item);
    if (ring_time_toast != null && item == ring_time_toast_alarm) {
        ring_time_toast_alarm = null;
        ring_time_toast.dismiss ();
    }
    show_delete_toast ();
    save ();
});

private void show_delete_toast () {
    if (deleted_alarm == null) {
        return;
    }

    if (ring_time_toast != null) {
        ring_time_toast.dismiss ();
    }

    var window = (Clocks.Window) get_root ();
    delete_toast = new Adw.Toast ("");

    delete_toast.set_title (_("Alarm deleted"));
    delete_toast.set_button_label (_("Undo"));
    delete_toast.button_clicked.connect (() => {
        if (deleted_alarm != null) {
            alarms.add (deleted_alarm);
            connect_item (deleted_alarm);
            deleted_alarm = null;
            save ();
            delete_toast.dismiss ();
        }
    });

    delete_toast.dismissed.connect (() => {
        deleted_alarm = null;
    });

    window.add_toast (delete_toast);
}
``` 

Essa solução melhora a experiência do usuário, tornando a deleção de alarmes mais segura e amigável, além de seguir boas práticas de UX ao oferecer uma forma de desfazer ações potencialmente destrutivas.

## Atividades realizadas

Durante a aula, realizei as seguintes atividades:

- Análise da issue sobre a falta de confirmação ao deletar alarmes
- Exploração do código fonte do GNOME Clocks
- Implementação da funcionalidade de desfazer deleção
- Testes da nova funcionalidade
- Investigação e tentativa de correção da issue de cidades duplicadas ([#329](https://gitlab.gnome.org/GNOME/gnome-clocks/-/issues/329))
- Comentário na issue relatando a possível origem do bug

A implementação da funcionalidade de desfazer deleção consistiu em:
1. Adicionar variáveis para armazenar o alarme deletado e o toast
2. Modificar a função de remoção para salvar o alarme
3. Implementar a função de mostrar o toast com opção de desfazer
4. Adicionar a lógica de restauração do alarme

## Reflexões

A experiência de trabalhar com Vala foi desafiadora, mas enriquecedora:

1. **Desafios Técnicos**
   - Aprendizado de uma nova linguagem
   - Adaptação ao estilo de código do projeto
   - Entendimento do sistema de toasts
   - Investigação de bugs em bibliotecas externas

2. **Aprendizados**
   - Gerenciamento de estado em aplicações GTK
   - Padrões de design do GNOME
   - Boas práticas de UX
   - Importância de analisar dependências externas

3. **Satisfação**
   - Implementação bem-sucedida da funcionalidade
   - Contribuição para melhorar a experiência do usuário
   - Superação dos desafios iniciais
   - Participação ativa na comunidade, mesmo sem resposta imediata

## Próximos passos

Com a implementação concluída e a investigação da issue de cidades duplicadas em andamento, os próximos passos incluem:

1. Responder a possíveis feedbacks
2. Continuar explorando outras issues do projeto
3. Acompanhar respostas sobre o possível bug na biblioteca gweather

A experiência de contribuir para o GNOME Clocks foi muito valiosa, especialmente por ter que aprender uma nova linguagem, trabalhar com um framework diferente e investigar problemas em bibliotecas externas. Estou animado para continuar contribuindo e aprendendo mais sobre o ecossistema GNOME.
