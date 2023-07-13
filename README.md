# DAX-Test


'''DAX
DEFINE
    TABLE Dates =
    GENERATE (
    CALENDAR ( DATE ( 2020, 1, 1 ), DATE ( 2020, 4, 30 ) ),
    VAR CurrentDate = [Date]
    RETURN
    ROW (
   
    // Date values
   
    "DateInt", 			FORMAT ( CurrentDate, "yyyymmdd" ), // Format: 20200101
    "DateShort", 		FORMAT ( CurrentDate, "dd/mm/yyyy" ), // Format: 01/01/2020
                    
    // Datetime values
    
    "DateTimeStart", 	[Date], // Format: 01/01/2020 00:00:00
    "DateTimeEnd",      [Date] + TIME ( 23, 59, 59 ), // Format: 01/01/2020 23:59:59
                    
    // Weekday values
    
    "WeekdayName", 		FORMAT ( CurrentDate, "dddd" ), // Format: Wednesday
    "WeekdayNameShort", FORMAT ( CurrentDate, "ddd" ), // Format: Wed 
                 
    // Start and end of week values (Monday and Sunday)
    
    "WeekStartDateMonday",
    FORMAT ( CurrentDate - WEEKDAY ( CurrentDate, 2 ) + 1, "dd/mm/yyyy" ), // Format: 30/12/2019
    "WeekEndDateSunday", 
    FORMAT ( CurrentDate - WEEKDAY ( CurrentDate, 2 ) + 7, "dd/mm/yyyy" ), // Format: 05/01/202        
       
    // Month values
   
    "MonthName", 		FORMAT ( [Date], "mmmm" ), // Format: January
    "MonthNameShort", 	FORMAT ( CurrentDate, "Mmm" ), // Format: Jan
    "MonthNumber", 		MONTH ( [Date] ), // Format: 1  
        
    // Start and end of month values
   
    "StartOfMonth", 	FORMAT(EOMONTH(CurrentDate, -1) +1, "dd/mm/yyyy"),  // Format: 01/01/2020		 				
    "EndOfMonth", 		FORMAT(EOMONTH(CurrentDate, 0), "dd/mm/yyyy"), // Format: 31/01/2020 
    
    // Year values 
  
    "CalendarYear",	   YEAR ( [Date] ), // Format: 2020
  	"FinancialYear",
  IF ( MONTH ( currentDate ) < 4, YEAR ( CurrentDate ) - 1, YEAR ( CurrentDate ) ) & " - "
  & IF ( MONTH ( currentDate ) < 4, YEAR ( currentDate ), YEAR ( currentDate ) + 1 ) // Format: 2019 - 2020
                )
        )

EVALUATE
SUMMARIZECOLUMNS (
    DATES[DateInt],
    DATES[DateShort],
    DATES[DateTimeStart],
    DATES[DateTimeEnd],
    DATES[WeekdayName],
    DATES[WeekdayNameShort],
    DATES[WeekStartDateMonday],
    DATES[WeekEndDateSunday],
    DATES[MonthName],
    DATES[MonthNameShort],
    DATES[MonthNumber],
    DATES[StartOfMonth],
    DATES[EndOfMonth],
    DATES[CalendarYear],
    DATES[FinancialYear]
)
'''
