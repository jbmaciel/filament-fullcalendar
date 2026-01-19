# Guia de Atualização para Filament v5 e Laravel 12+

Este documento descreve todas as mudanças realizadas no projeto `filament-fullcalendar` para atualizar a compatibilidade com **Filament v5** e **Laravel 12+**.

## Mudanças Realizadas

### 1. composer.json

#### Dependências do Filament
- **Antes**: `"filament/filament": "^4.0"`
- **Depois**: `"filament/filament": "^5.0"`

#### Dependências do Laravel
- **Antes**: `"illuminate/contracts": "^10.0|^11.0|^12.0"`
- **Depois**: `"illuminate/contracts": "^11.0|^12.0"`

**Motivo**: Filament v5 requer Laravel 11+. O Laravel 10 já está fora de suporte. A versão mínima do PHP permanece em `^8.2`.

### 2. src/Actions/ViewAction.php

#### Remoção do método `modalFooterActions()`
O método `modalFooterActions()` foi removido desta ação. No Filament v5, o gerenciamento de actions no modal é feito de forma diferente.

**Antes**:
```php
$this->modalFooterActions(
    fn (ViewAction $action, FullCalendarWidget $livewire) => [
        ...$livewire->getCachedFormActions(),
        $action->getModalCancelAction(),
    ]
);
```

**Depois**: Removido completamente. O modal usa o comportamento padrão do Filament v5.

**Motivo**: A API de Actions do Filament v5 foi refatorada para maior consistência e simplicidade.

## Arquivos que NÃO Necessitaram de Alterações

Os seguintes arquivos estão **totalmente compatíveis** com Filament v5 sem necessidade de modificações:

- ✅ `src/FilamentFullCalendarPlugin.php` - Continua implementando `Plugin` corretamente
- ✅ `src/FilamentFullCalendarServiceProvider.php` - Compatível com o novo sistema de assets
- ✅ `src/Widgets/FullCalendarWidget.php` - A implementação de traits está correta
- ✅ `src/Widgets/Concerns/InteractsWithRecords.php` - Implementação de records compatible
- ✅ `src/Widgets/Concerns/InteractsWithRawJS.php` - Sem mudanças necessárias
- ✅ `src/Widgets/Concerns/CanBeConfigured.php` - Sem mudanças necessárias
- ✅ `src/Widgets/Concerns/IsBackwardCompatible.php` - Sem mudanças necessárias
- ✅ `src/Widgets/Concerns/InteractsWithEvents.php` - Sem mudanças necessárias
- ✅ `src/Actions/CreateAction.php` - Compatível com v5
- ✅ `src/Actions/EditAction.php` - Compatível com v5
- ✅ `src/Actions/DeleteAction.php` - Compatível com v5
- ✅ `src/Data/EventData.php` - Sem mudanças necessárias
- ✅ `src/helpers.php` - Sem mudanças necessárias
- ✅ `resources/views/fullcalendar.blade.php` - Compatível com views do Filament v5
- ✅ `package.json` - Dependências JavaScript já estão atualizadas para FullCalendar v6+

## Possíveis Mudanças Futuras

### Observações Importantes

1. **Teste Completo Recomendado**: Embora o código esteja compatível, é recomendado testar completamente as seguintes funcionalidades:
   - Criar, editar e deletar eventos
   - Drag and drop de eventos
   - Resize de eventos
   - Configurações de timezone e locale

2. **Métodos Obsoletos**: Verifique se há algum método deprecado em suas aplicações que usam este plugin:
   - O trait `IsBackwardCompatible` fornece suporte para métodos antigos (`headerActions()` e `modalActions()`)
   - Recomenda-se migrar para os novos métodos do Filament v5 quando possível

3. **Assets**: O carregamento de assets Alpine está compatível com o novo sistema de `FilamentAsset::register()` do Filament v5

## Versões Suportadas

Após esta atualização, o plugin suporta:

- **Laravel**: 11.x e 12.x
- **Filament**: v5.x
- **PHP**: 8.2+
- **FullCalendar**: 6.x

## Instalação

Para atualizar seu projeto:

```bash
composer require "filament/filament:^5.0" "illuminate/contracts:^11.0|^12.0"
```

Ou simplesmente:

```bash
composer update
```

## Rollback para Filament v4

Se precisar voltar para Filament v4, reverta as mudanças no `composer.json`:

```json
{
    "filament/filament": "^4.0",
    "illuminate/contracts": "^10.0|^11.0|^12.0"
}
```

E adicione de volta o código no `src/Actions/ViewAction.php`:

```php
$this->modalFooterActions(
    fn (ViewAction $action, FullCalendarWidget $livewire) => [
        ...$livewire->getCachedFormActions(),
        $action->getModalCancelAction(),
    ]
);
```

## Contribuição

Para reportar problemas de compatibilidade, abra uma issue no repositório GitHub.
