# AppHotel

Aplicativo de simulação de reserva de hotel desenvolvido em .NET MAUI. Permite selecionar tipo de suíte, número de hóspedes e datas para calcular diárias.

## Funcionalidades

- Seleção do tipo de quarto (Super Luxo, Luxo, Single, Crise) via `Picker`
- Definição do número de adultos e crianças via `Stepper`
- Escolha das datas de check-in e check-out via `DatePicker`
- Cálculo do valor da diária com base nas escolhas
- Navegação para tela de resumo da contratação

## Como funciona

O app possui uma tela principal (`ContratacaoHospedagem`) com:

- **Steppers** (`stp_adultos`, `stp_criancas`) vinculados a `Label` via `BindingContext="{x:Reference stp_adultos}"` e `Text="{Binding Value}"` — atualização em tempo real sem código C#.
- **Picker** (`pck_quarto`) populado com `App.lista_quartos` (lista estática de 4 tipos de quarto com valores de diária para adulto e criança).
- **DatePickers** com restrições: check-in mínimo = hoje, máximo = +1 mês; check-out mínimo = check-in + 1 dia, máximo = check-in + 2 meses.
- **Botão "Avançar"** que navega para `HospedagemContratada` via `Navigation.PushAsync`.

### Estrutura do Projeto

```
Models/Quarto.cs                          → Modelo com Descricao, ValorDiariaAdulto, ValorDiariaCrianca
Views/ContratacaoHospedagem.xaml(.cs)     → Tela de formulário de reserva
Views/HospedagemContratada.xaml(.cs)      → Tela de resumo (stub)
```

## Conceitos novos aprendidos

1. **BindingContext com x:Reference** — vinculação de elementos na mesma página sem code-behind: um `Label` referencia um `Stepper` via `BindingContext="{x:Reference stp_adultos}"` e exibe seu `Value`.
2. **Stepper** — controle de entrada numérica incremental com intervalo definido (`Minimum`, `Maximum`, `Increment`).
3. **DatePicker com restrições dinâmicas** — `MinimumDate` e `MaximumDate` ajustados programaticamente no evento `DateSelected` para evitar datas inválidas.
4. **Picker com ItemDisplayBinding** — vinculação de uma lista de objetos a um `Picker`, exibindo uma propriedade específica (`Descricao`) como texto visível.
5. **Static global state** — dados compartilhados via `public static List<Quarto>` em `App.xaml.cs`, acessível de qualquer página como `App.lista_quartos`.
6. **Passagem de dados entre páginas via BindingContext** — conceito de definir `BindingContext = objeto` no construtor da página destino para exibir dados recebidos.
7. **Navegação Push/Pop** — `Navigation.PushAsync` para empilhar páginas e `Navigation.PopAsync` para retornar.
8. **Custom fonts (Kalam)** — registro e uso de fontes personalizadas além das padrão do MAUI.
