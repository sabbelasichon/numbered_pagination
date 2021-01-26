# TYPO3 Extension `numbered_pagination`

Since TYPO3 10 a new pagination API is shipped which supersedes the pagination widget controller which is removed in version 11.0.
[Official documentation](https://docs.typo3.org/m/typo3/reference-coreapi/master/en-us/ApiOverview/Pagination/Index.html)

This extension provides an improved pagination which can be used to paginate array items or query results from Extbase.
The main advantage is that it reduces the amount of pages shown.

**Example**: Imagine 1000 records and 20 items per page which would lead to 50 links.
Using the `NumberedPagination`, you will get something like `< 1 2 ... 21 22 23 24 ... 100 >`

## Installation

Install the extension with `composer require georgringer/numbered_pagination` or by downloading it
from [extensions.typo3.org](https://extensions.typo3.org/extension/numbered_pagination/) or the Extension Manager.

## Usage

Just replace the usage of `SimplePagination` with `\GeorgRinger\NumberedPagination\Pagination\NumberedPagination` and you are done.
Set the 2nd argument to the maximum number of links which should be rendered.

```php
$currentPage = $this->request->hasArgument('currentPage') ? (int)$this->request->getArgument('currentPage') : 1;
$paginator = new \TYPO3\CMS\Extbase\Pagination\QueryResultPaginator($allItems, $currentPage, 20);
$pagination = new \GeorgRinger\NumberedPagination\NumberedPagination($paginator, 10);
$this->view->assign('pagination', [
    'paginator' => $paginator,
    'pagination' => $pagination,
]);
```

### Templating

```html
<f:for each="{pagination.paginator.paginatedItems}" as="item" iteration="iterator">
    <f:render partial="item" arguments="{item:item}"/>
</f:for>
<f:render partial="Pagination" arguments="{pagination: pagination.pagination, paginator: pagination.paginator}" />
```

Copy the pagination partial `EXT:numbered_pagination/Resources/Private/Partials/Pagination.html` to your extension or use it directly by providing the path mapping:

```typo3_typoscript
# Example for extension "fo"
plugin.tx_fo.view.partialRootPaths.4483 = EXT:numbered_pagination/Resources/Private/Partials/
```
