# Handling-calendar-and-presence-spreadsheet-for-google-form

This Google Apps Script automates calendar event creation and monthly attendance tracking for a dog boarding/grooming business using a Google Spreadsheet and Google Calendar.
📌 What It Does

    Adds Events to Google Calendar

        Creates check-in and check-out events for each dog based on arrival and departure dates, Differenciating between grooming and not grooming at         departure.

        Updates existing events if the dates change.

        Archives rows with past departures into an "Archivio" sheet.

    Fills Monthly Sheets

        Automatically fills out a row in monthly attendance sheets (Presenze Gennaio, Presenze Febbraio, etc.) to track presence across days.

        Colors cells to show check-ins (green), check-outs (blue or red), and washing status.

    Processes Only Once

        Uses flags in columns to avoid reprocessing already handled rows (Event Created, Presence saved).

✅ How to Use

    Link google form for reservation to a Google Spreadsheet, link google App script to spreadsheet

        Columns must be ordered exactly as expected (see notes below).

        Required sheets: active sheet with bookings, optional "Archivio", monthly sheets will be created automatically.

    Authorize the Script

        First-time use will prompt Google authorization to access your Calendar and Sheets.

    Add Calendar ID

        Update this line in addToCalendar() with the correct Calendar ID:

        const calendar = CalendarApp.getCalendarById('your-email@gmail.com');

    Trigger the Script

        Use the main() function to process both calendar events and monthly attendance.

        You can set a time-driven trigger in the Apps Script UI to run main() daily.

📊 Expected Form/Spreadsheet Format
Col #	Column Name	Notes
1	(Unused)	
2	Email	Client's email
3	Name	Client's name
4	Address	Client's address
5	Phone	Client's phone number
6	Dog Name	Name of the dog
7	Breed/Size/Age	Free text, but breed must be first word
8	Wash	"Sì" for yes, "No" or blank otherwise
9	Sex	"Maschio" or "Femmina"
10	Arrival Date	Format: dd/mm/yyyy hh:mm or Date object
11	Departure Date	Format: dd/mm/yyyy hh:mm or Date object
...	(Unused)	
16	Status	"Event Created" when calendar entry is made
17	Stored Arrival Date	Used to detect changes
18	Stored Departure Date	Used to detect changes
19	(Unused)	
20	Presence Status	"Presence saved" when monthly sheets are updated
⚠️ Important Notes & Pitfalls

    Column Order Is Critical: If the form and therefore the spreadsheet structure changes, the script may fail or behave incorrectly.

    Date Formatting Must Be Consistent: Dates should be parsed correctly by JavaScript. Using actual Date objects in Google Sheets is recommended.

    Color Coding:

        Arrival: Green (#93c47d)

        Departure with Wash: Light Blue (#a6bfe7)

        Departure without Wash: Pink (#f4afaf)

    Archiving: Past bookings (departure date before "now") are removed from the main sheet and stored in the "Archivio" sheet.

🔧 Customization Tips

    Want different calendar event colors? Modify:

    CalendarApp.EventColor.BLUE
    CalendarApp.EventColor.GREEN

    Need to change how dog info is parsed? Edit getRazza() to adjust parsing from column 7.

📅 Triggers Recommendation

To keep your calendar and attendance up to date:
    Recomended trigger is set at every google form arrival, also set a daily time-based trigger to remain up to date. Avoid trigger by modification       of the script, because if could behave inconsistently.

