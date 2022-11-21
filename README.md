# How to load appointments On-Demand in Flutter Calendar?

This example demonstrates how to load appointments On-Demand in Flutter Calendar.

By using the handleLoadMore method you can load the appointments to the Flutter Event Calendar on-demand and use the [loadMoreWidgetBuilder](https://pub.dev/documentation/syncfusion_flutter_calendar/latest/calendar/SfCalendar/loadMoreWidgetBuilder.html) to customise the loading indicator.

## Define Load more widget builder.

You can build your own custom widget by using the loadMoreWidgetBuilder that will be displayed as a loading indicator in the calendar when the calendar view changes.

```
Widget loadMoreWidget(
    BuildContext context, LoadMoreCallback loadMoreAppointments) {
  return FutureBuilder<void>(
    initialData: 'loading',
    future: loadMoreAppointments(),
    builder: (context, snapShot) {
      return Container(
          alignment: Alignment.center, child: CircularProgressIndicator());
    },
  );
}

```

## Loading appointments
Update the appointments on-demand, when the loading indicator is displaying in the calendar by using the [handleLoadMore](https://pub.dev/documentation/syncfusion_flutter_calendar/latest/calendar/CalendarDataSource/handleLoadMore.html) method in the CalendarDataSource.


```
@override
Future<void> handleLoadMore(DateTime startDate, DateTime endDate) async {
  await Future<void>.delayed(const Duration(seconds: 1));
  List<_Meeting> meetings = <_Meeting>[];
  DateTime date = DateTime(startDate.year, startDate.month, startDate.day);
  DateTime appEndDate =
  DateTime(endDate.year, endDate.month, endDate.day, 23, 59, 59);
  while (date.isBefore(appEndDate)) {
    final List<_Meeting>? data = _dataCollection[date];
    if (data == null) {
      date = date.add(const Duration(days: 1));
      continue;
    }

    for (final _Meeting meeting in data) {
      if (appointments.contains(meeting)) {
        continue;
      }

      meetings.add(meeting);
    }
    date = date.add(const Duration(days: 1));
  }

  appointments.addAll(meetings);
  notifyListeners(CalendarDataSourceAction.add, meetings);;
}

```

You can also refer our UG documentation to know more about [LoadMore](https://help.syncfusion.com/flutter/calendar/load-more) support in the Flutter Calendar.

## Requirements to run the demo
* [VS Code](https://code.visualstudio.com/download)
* [Flutter SDK v1.22+](https://flutter.dev/docs/development/tools/sdk/overview)
* [For more development tools](https://flutter.dev/docs/development/tools/devtools/overview)

## How to run this application
To run this application, you need to first clone or download the ‘create a flutter maps widget in 10 minutes’ repository and open it in your preferred IDE. Then, build and run your project to view the output.

## Further help
For more help, check the [Syncfusion Flutter documentation](https://help.syncfusion.com/flutter/introduction/overview),
 [Flutter documentation](https://flutter.dev/docs/get-started/install).