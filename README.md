# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked 'enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file
2.CSV file should have valid readings.
3.Read access for the csv file for computation i.e not locked.
4.Permission to generate and store the PDF report in the server.
5.Accessing the system date and time to send notification on weekly basis.
6.Library to get date and time(from system clock), PDF generator/converter.
7.Notification detail :whom it needs to be notified and notification type email,sms,console etc.,.


### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No            |Third party libraries or APIs.which would have already been tested
Counting the breaches       | Yes           | To count the number of breaches encountered every month
Detecting trends            | Yes           | This is part of the software being developed.Date & time need to be stored when the reading was continuously increasing for 30 minutes 
Notification utility        | Yes           |This is part of the software being developed.We only test if the notification method is being called by using mock.


### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings
2. Write "Invalid input" to the PDF when the csv doesn't contain expected data
3.Write "No Data Found to process" to the PDF when the csv is empty.
4.Write "File Not Found" when csv file if not found on server.
5.Duplicate entries should not be allowed to avoid inaccuracy in the analysis generated. If exists then it should be deleted automatically.
6.Write "Count of breaches:{breachcount}" of an attribute captured in a month to the PDF when the CSV have some reading which are crossing threshold.
7.Write "No breaches found" to the PDF when no breaches captured for the month.
8.Write "Record Trends"  along with system date and time to the PDF if the trends increase continuously if its been 30 minutes.
9.Write "No Record Trends Available" in trends when there is no continuous reading increase available in CSV File.
10.Return "Report Generation Failure" when the pdf file is not generated for the week.
11.Return "Report Generation Success" when the generated PDF file is stored/saved in specified location of server every week.
12.Whenever a new pdf is generation is successfull, check if notification is invoked.
13.In case of mail notification "Sender/Recepient Invalid" when sender/recepient email address are invalid or not found.
14.Return "Email Sent/error Return code" whenever an email is sent or not for the available report.

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | PDF file     | invocation of notification utility if report is available               | mock the notification utility
Report inaccessible server |server report path|Access denied with response code|Fake the server access
Find minimum and maximum   | csv data |  mininum or maximum  value               | None - it's a pure function
Detect trend               | csv data | Record Trends(trend measured with timestamp)/No Record Trends              | None - it's a pure function
Write to PDF               | csv processed data | PDF file               |Mocks the pdf creation

## Notify report availability

Since the invocation of notification has a dependency on pdf report creation, whenever the notification service triggers any one of the service type(example:email or sms service)we don't want to send the notification each time we run notification test.
However,we want to verify that the corresponding service is called.Such a service can be replaced with a mock object.

## Write to PDF

Creation of Pdf has an external dependency to a third party libraries or APIs.Often those depenencies may themselves be under development.Without mocking,if a testcase fails,it will be hard to know if the failure is due to ourcode unit or due to dependencies.Mocking therefore speeds up development and testing by isolating failures.
    
