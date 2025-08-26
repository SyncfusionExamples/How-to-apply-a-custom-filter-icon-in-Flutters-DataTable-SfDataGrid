# How-to-apply-a-custom-filter-icon-in-Flutters-DataTable-SfDataGrid

In this article, we will show you how to customize the filter icon in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).


Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required properties. The [onFilterChanged](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/onFilterChanged.html) callback is triggered when a column filter is applied via the UI, providing the column name along with filter details such as type and value. By using a flag map to track each column's filter state, you can enable validation and customize the filter icon display based on the filtering status.

```dart
// Stores the filter state for each column by its name
Map<String, bool> filterStates = {};

@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Syncfusion Flutter DataGrid')),
      body: SfDataGridTheme(
        data: SfDataGridThemeData(
          filterIcon: Builder(
            builder: (context) {
              Widget? icon;
              String columnName = '';
              context.visitAncestorElements((element) {
                if (element is GridHeaderCellElement) {
                  columnName = element.column.columnName;
                }
                return true;
              });

              // Checks if a filter is applied to the selected column
              bool column = filterStates[columnName] ?? false;

              if (column) {
                icon = const Icon(
                  Icons.filter_alt_outlined,
                  size: 20,
                  color: Colors.purple,
                );
              }
              return icon ??
                  const Icon(
                    Icons.filter_alt_off_outlined,
                    size: 20,
                    color: Colors.deepOrange,
                  );
            },
          ),
        ),
        child: SfDataGrid(
          source: employeeDataSource,
          columnWidthMode: ColumnWidthMode.fill,
          allowFiltering: true,
          onFilterChanged: (details) {
            final columnName = details.column.columnName;
            final isFilterActive = details.filterConditions.isNotEmpty;
            // Updates _filterStates using the details from the onFilterChanged callback
            setState(() {
              filterStates[columnName] = isFilterActive;
            });
          },
          columns: <GridColumn>[
            GridColumn(
              columnName: 'id',
              label: Container(
                padding: EdgeInsets.all(16.0),
                alignment: Alignment.center,
                child: Text('ID'),
              ),
            ),
            GridColumn(
              columnName: 'name',
              label: Container(
                padding: EdgeInsets.all(8.0),
                alignment: Alignment.center,
                child: Text('Name'),
              ),
            ),
            GridColumn(
              columnName: 'designation',
              label: Container(
                padding: EdgeInsets.all(8.0),
                alignment: Alignment.center,
                child: Text('Designation', overflow: TextOverflow.ellipsis),
              ),
            ),
            GridColumn(
              columnName: 'salary',
              label: Container(
                padding: EdgeInsets.all(8.0),
                alignment: Alignment.center,
                child: Text('Salary'),
              ),
            ),
          ],
        ),
      ),
    );
  }

```

You can download this example in [GitHub](https://github.com/SyncfusionExamples/How-to-apply-a-custom-filter-icon-in-Flutters-DataTable-SfDataGrid.git).
