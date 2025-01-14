<img src="https://github.com/user-attachments/assets/a44f93c5-86b2-4a5c-bcb3-d48d7104fd3f" width="100%" alt="Preview" class="filament-hidden" />


# List Entry (Filament Infolist Plugin)

[![Latest Version on Packagist](https://img.shields.io/packagist/v/fauzie811/filament-list-entry.svg?style=flat-square)](https://packagist.org/packages/fauzie811/filament-list-entry)
[![GitHub Tests Action Status](https://img.shields.io/github/actions/workflow/status/fauzie811/filament-list-entry/run-tests.yml?branch=main&label=tests&style=flat-square)](https://github.com/fauzie811/filament-list-entry/actions?query=workflow%3Arun-tests+branch%3Amain)
[![GitHub Code Style Action Status](https://img.shields.io/github/actions/workflow/status/fauzie811/filament-list-entry/fix-php-code-style-issues.yml?branch=main&label=code%20style&style=flat-square)](https://github.com/fauzie811/filament-list-entry/actions?query=workflow%3A"Fix+PHP+code+style+issues"+branch%3Amain)
[![Total Downloads](https://img.shields.io/packagist/dt/fauzie811/filament-list-entry.svg?style=flat-square)](https://packagist.org/packages/fauzie811/filament-list-entry)


Plugin for FilamentPHP v3.


## Installation

You can install the package via composer:

```bash
composer require fauzie811/filament-list-entry
```

## Usage

Use it in your Infolist section.

```php
// use Fauzie811\FilamentListEntry\Infolists\Components\ListEntry;

    public static function infolist(Infolist $infolist): Infolist
    {
        return $infolist
            ->schema([
                ListEntry::make('users')
                    ->label('Default with Icon')
                    ->itemIcon('heroicon-o-check'),
            ]);
    }
```

All methods:

* Generic:
    * ```->label('Define top section label')```
    * ```->inline(true)``` change the listStyle to inline. Activate separator.
    * ```->badge(true)``` activate the badge for each line. Desactivate itemActions, itemDescription
    * ```->separator(',')``` change the separator, by default ```, ``` (coma space)
    * ```->getStateUsing(['a', 'b', 'c'])``` specify manually the data to be used oterwize use the relationship
    * ```->emptyStateEnabled(true)``` activate or not the Empty State. Default true
    * ```->emptyStateHeading('No data')``` change the Heading of the Empty State
    * ```->emptyStateDescription('There is nothing')``` change the Description of the Empty State
    * ```->emptyStateIcon('heroicon-o-x-mark')``` change the Icon of the Empty State

* Record specific (all are Closure compatible):
    * ```->itemLabel(fn ($record) => $record->item)``` specify the label. By default, will try to stringify the record
    * ```->itemDescription(fn ($record) => sprintf('Percentage: %s%%', $record['score'] * 100))``` add description under the label
    * ```->itemIcon(fn($record) => 'heroicon-o-check')``` define an icon.
    * ```->itemIconColor(fn($record) => 'warning')``` define a color for the icon.
    * ```->itemUrl(fn($record) => '#')``` define a link if the user click on the icon, label or description.
    * ```->itemActions(fn($record) => ...)``` define Actions and ActionGroups at the right of the line. See Filament Actions documentation.

## List of examples

<table>
    <tr>
        <td> Example </td> <td> Code </td>
    </tr>
    <!-- Example 1 -->
    <tr>
        <td valign="top">
            Default with Icon

![image](https://github.com/user-attachments/assets/1d615a3f-22f6-4d4e-b732-d52be8c5cb0b)
        </td>
        <td>

```php
ListEntry::make('')
    ->label('Default with Icon')
    ->getStateUsing(['a', 'b', 'c'])
    ->itemIcon('heroicon-o-check'),
```
</td>
    </tr>
    <!-- Example 2 -->
    <tr>
        <td valign="top">
            Inline badges list with icons & links

![image](https://github.com/user-attachments/assets/97966b09-e709-4901-8e83-9aeae7dc9338)
        </td>
        <td>

```php
ListEntry::make('')
    ->inline(true)
    ->label('Inline badge with icon & link')
    ->getStateUsing(['a', 'b', 'c'])
    ->itemIcon('heroicon-o-check')
    ->itemUrl(fn ($record) => '#' . $record)
    ->badge(true),
```
</td>
    </tr>
    <!-- Example -->
    <tr>
        <td valign="top">
            Inline list with custom separator

![image](https://github.com/user-attachments/assets/612e49d5-9b5c-4d31-b005-a9e0af1c4dba)
        </td>
        <td>

```php
ListEntry::make('')
    ->listStyle('inline')
    ->label('inline simple +')
    ->getStateUsing(['a', 'b', 'c'])
    ->separator(' + '),
```
</td>
    </tr>
    <!-- Example -->
    <tr>
        <td valign="top">
            Inline list with Icon

![image](https://github.com/user-attachments/assets/74a7f1d5-eb76-47b9-a4be-8086bc739849)
        </td>
        <td>

```php
ListEntry::make('')
    ->listStyle('inline')
    ->label('inline with Icon')
    ->getStateUsing(['a', 'b', 'c'])
    ->itemIcon('heroicon-o-check'),
```
</td>
    </tr>
    <!-- Example -->
    <tr>
        <td valign="top">
            Complex list with actions

![image](https://github.com/user-attachments/assets/805acc96-f84e-4198-86a4-0c3bccd11fa1)
        </td>
        <td>

```php
ListEntry::make('scoresTop5')
    ->listStyle('list')
    ->itemLabel(fn ($record) => $record->item)
    ->itemUrl(fn ($record) => '#Url-' . $record->id)
    ->itemActions(
        fn ($record) => ActionGroup::make([
            Action::make('view'),
            Action::make('edit'),
            Action::make('delete'),
        ])
            ->size(ActionSize::Small)
    ),
```
</td>
    </tr>
    <!-- Example -->
    <tr>
        <td valign="top">
            Complex list with custom data, and all options

![image](https://github.com/user-attachments/assets/f12a1ae4-8897-476c-92b5-839fede594cd)
        </td>
        <td>

```php
ListEntry::make('checklist')
    ->listStyle('list')
    ->getStateUsing([
        ['name' => 'Complete profile #1', 'score' => 1],
        ['name' => 'Complete profile #2', 'score' => .75]
    ])
    ->itemIcon(fn ($record) => match (true) {
        $record['score'] >= 1 => 'heroicon-o-check',
        default => 'heroicon-o-exclamation-triangle'
    })
    ->itemIconColor(fn ($record) => match (true) {
        $record['score'] >= 1 => 'success',
        default => 'danger'
    })
    ->itemActions(
        fn ($record) => [
            ViewAction::make('view1')
                    ->url('#View1-' . $record['name']),
            ActionGroup::make([
                Action::make('view2')
                    ->url('#View2-' . $record['name']),
                Action::make('edit'),
                Action::make('delete'),
            ])
                ->size(ActionSize::Small)
        ]
    )
    ->itemUrl(fn ($record) => '#Url-' . $record['name'])
    ->itemLabel(fn ($record) => $record['name'])
    ->itemDescription(function ($record) {
      return sprintf('Percentage: %s%%', $record['score'] * 100)
    }),
```
</td>
    </tr>
</table>

## Dark mode support

This plugin is compatible with Light mode and Dark mode.

![light mode](https://github.com/user-attachments/assets/33364a4d-4995-4beb-bfdc-2f38ab8d89aa)
![dark mode](https://github.com/user-attachments/assets/7097edf1-f8d7-4860-a4bf-797229607687)


## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

(Do not hesitate to contribute !)

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Thiktak](https://github.com/Thiktak)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
