# DAX-Test


```

Now that the relevant assembly is in place, running the script below will create the corresponding function that allows it to be used within the database.

```TSQL
IF OBJECT_ID('[NumberAfterString]') IS NOT NULL
DROP FUNCTION [NumberAfterString]
GO
CREATE FUNCTION [NumberAfterString]
(
  @string_in NVARCHAR(MAX),
  @pattern NVARCHAR(255)
)
RETURNS NVARCHAR(255)
WITH EXECUTE AS CALLER
AS 
EXTERNAL NAME [NumberAfterString].[CLRFunction].[NumberAfterString]
GO

```


```DAX
coeff corr = 
var __muX =calculate(AVERAGE(CorrelHeadSizeBrainWeight[Head_Size]))
var __muY=calculate(AVERAGE(CorrelHeadSizeBrainWeight[Brain_Weight]))
var __numerator  =  sumx('CorrelHeadSizeBrainWeight',( [x]-__muX)*([y]-__muY))
var __denominator=  SQRT(sumx('CorrelHeadSizeBrainWeight',([x]-__muX)^2)*sumx('CorrelHeadSizeBrainWeight',([y]-__muY)^2))
return 
divide(__numerator,__denominator)

Skewness = 
VAR __mean = AVERAGEX(ALL('Data'),[Values])
VAR __stddev = STDEVX.P(ALL('Data'),[Values])
VAR __n = COUNTROWS(ALL('Data'))
VAR __table = ADDCOLUMNS('Data',"__skew",POWER(([Values]-__mean),3))
VAR __sum = SUMX(__table,[__skew])
RETURN
DIVIDE(__sum,POWER(__stddev,3),0) * DIVIDE(1,__n,0)

Simple linear regression =
VAR Known =
    FILTER (
        SELECTCOLUMNS (
            ALLSELECTED ( 'Date'[Date] ),
            "Known[X]", 'Date'[Date],
            "Known[Y]", [Measure Y]
        ),
        AND (
            NOT ( ISBLANK ( Known[X] ) ),
            NOT ( ISBLANK ( Known[Y] ) )
        )
    )
VAR SlopeIntercept =
    LINESTX(Known, Known[Y], Known[X])
VAR Slope =
    SELECTCOLUMNS(SlopeIntercept, [Slope1])
VAR Intercept = 
    SELECTCOLUMNS(SlopeIntercept, [Intercept])
RETURN
    SUMX (
        DISTINCT ( 'Date'[Date] ),
        Intercept + Slope * 'Date'[Date]
    )

```
