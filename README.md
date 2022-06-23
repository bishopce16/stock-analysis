# stock-analysis

## Overview of Project
### Purpose 
Using Visual Basic Application or VBA to analysis green stock for Steve, to discover which stocks are worth the investment for his parents. To accomplished this, we had to find the stock’s ticker, total daily volume and annual return value in the year 2017 and 2018. We created a run button to support Steve in future projects, as well as refactoring the original code to improve its productivity. 

## Results 
### Refactoring the Green Stock Analysis Code
Refactoring the code involved first copying the original code and changing the nested loops and creating four different arrays: tickers, tickerVolumes, tickerStartingPrices, and tickerEndingPrices.

#### Refactored Code
    Sub AllStocksAnalysisRefactored()
        Dim startTime As Single
        Dim endTime  As Single

        yearValue = InputBox("What year would you like to run the analysis on?")

        startTime = Timer
        
        'Format the output sheet on All Stocks Analysis worksheet
        Worksheets("All Stocks Analysis").Activate
        
        Range("A1").Value = "All Stocks (" + yearValue + ")"
        
        'Create a header row
        Cells(3, 1).Value = "Ticker"
        Cells(3, 2).Value = "Total Daily Volume"
        Cells(3, 3).Value = "Return"

        'Initialize array of all tickers
        Dim tickers(12) As String
        
        tickers(0) = "AY"
        tickers(1) = "CSIQ"
        tickers(2) = "DQ"
        tickers(3) = "ENPH"
        tickers(4) = "FSLR"
        tickers(5) = "HASI"
        tickers(6) = "JKS"
        tickers(7) = "RUN"
        tickers(8) = "SEDG"
        tickers(9) = "SPWR"
        tickers(10) = "TERP"
        tickers(11) = "VSLR"
        
        'Activate data worksheet
        Sheets(yearValue).Activate
        
        'Get the number of rows to loop over
        RowCount = Cells(Rows.Count, "A").End(xlUp).Row
        
        '1a) Create a ticker Index
        
        tickerIndex = 0

        '1b) Create three output arrays
        Dim tickerVolumes(12) As Long
        Dim tickerStartingPrices(12) As Single
        Dim tickerEndingPrices(12) As Single
        
        
        '2a) Create a for loop to initialize the tickerVolumes to zero.
        For i = 0 To 11
            tickerVolumes(i) = 0
        
            
        Next i
        
            
        '2b) Loop over all the rows in the spreadsheet.
        For i = 2 To RowCount
        
            '3a) Increase volume for current ticker
            tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
            
    
            
            '3b) Check if the current row is the first row with the selected tickerIndex.
            'If  Then
            If Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
                    tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
                
            End If
                
                
            'End If
            
            '3c) check if the current row is the last row with the selected ticker
            'If the next row‚Äôs ticker doesn‚Äôt match, increase the tickerIndex.
            'If  Then
            If Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            
            
                

                '3d Increase the tickerIndex.
                
                tickerIndex = tickerIndex + 1
                
            End If
            'End If
        
        Next i
        
        '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
        For i = 0 To 11
            
            Worksheets("All Stocks Analysis").Activate
            tickerIndex = i
            Cells(4 + i, 1).Value = tickers(tickerIndex)
            Cells(4 + i, 2).Value = tickerVolumes(tickerIndex)
            Cells(4 + i, 3).Value = tickerEndingPrices(tickerIndex) / tickerStartingPrices(tickerIndex) - 1
            
            
        Next i
        
        'Formatting
        Worksheets("All Stocks Analysis").Activate
        Range("A3:C3").Font.FontStyle = "Bold"
        Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
        Range("B4:B15").NumberFormat = "#,##0"
        Range("C4:C15").NumberFormat = "0.0%"
        Columns("B").AutoFit

        dataRowStart = 4
        dataRowEnd = 15

        For i = dataRowStart To dataRowEnd
            
            If Cells(i, 3) > 0 Then
                
                Cells(i, 3).Interior.Color = vbGreen
                
            Else
            
                Cells(i, 3).Interior.Color = vbRed
                
            End If
            
        Next i
    
        endTime = Timer
        MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

    End Sub



#### Original Code
    Sub MacroCheck()
        Dim testMessage As String
        testMessage = "Hello World!"
        MsgBox (testMessage)
        
    
    End Sub

    Sub DQAnalysis()
        Worksheets("DQ Analysis").Activate

        Range("A1").Value = "DAQO (Ticker: DQ)"

        'Create a header row
        Cells(3, 1).Value = "Year"
        Cells(3, 2).Value = "Total Daily Volume"
        Cells(3, 3).Value = "Return"
        Worksheets("2018").Activate

        'set initial volume to zero
        totalVolume = 0

        Dim startingPrice As Double
        Dim endingPrice As Double

        'Establish the number of rows to loop over
        rowStart = 2
        rowEnd = Cells(Rows.Count, "A").End(xlUp).Row

        'loop over all the rows
        For i = rowStart To rowEnd

            If Cells(i, 1).Value = "DQ" Then

                'increase totalVolume by the value in the current row
                totalVolume = totalVolume + Cells(i, 8).Value

            End If

            If Cells(i - 1, 1).Value <> "DQ" And Cells(i, 1).Value = "DQ" Then

                startingPrice = Cells(i, 6).Value

            End If

            If Cells(i + 1, 1).Value <> "DQ" And Cells(i, 1).Value = "DQ" Then

                endingPrice = Cells(i, 6).Value

            End If

        Next i

        Worksheets("DQ Analysis").Activate
        Cells(4, 1).Value = 2018
        Cells(4, 2).Value = totalVolume
        Cells(4, 3).Value = (endingPrice / startingPrice) - 1


    End Sub


    Sub AllStocksAnalysis()

        Dim startTime As Single
        Dim endTime  As Single

        yearValue = InputBox("What year would you like to run the analysis on?")

        startTime = Timer
    '1) Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    '2) Initialize array of all tickers
    Dim tickers(11) As String
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    '3a) Initialize variables for starting price and ending price
    Dim startingPrice As Single
    Dim endingPrice As Single
    '3b) Activate data worksheet
    Worksheets(yearValue).Activate
    '3c) Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row

    '4) Loop through tickers
    For i = 0 To 11
        ticker = tickers(i)
        totalVolume = 0
        '5) loop through rows in the data
        Worksheets(yearValue).Activate
        For j = 2 To RowCount
            '5a) Get total volume for current ticker
            If Cells(j, 1).Value = ticker Then

                totalVolume = totalVolume + Cells(j, 8).Value

            End If
            '5b) get starting price for current ticker
            If Cells(j - 1, 1).Value <> ticker And Cells(j, 1).Value = ticker Then

                startingPrice = Cells(j, 6).Value

            End If

            '5c) get ending price for current ticker
            If Cells(j + 1, 1).Value <> ticker And Cells(j, 1).Value = ticker Then

                endingPrice = Cells(j, 6).Value

            End If
        Next j
        '6) Output data for current ticker
        Worksheets("All Stocks Analysis").Activate
        Cells(4 + i, 1).Value = ticker
        Cells(4 + i, 2).Value = totalVolume
        Cells(4 + i, 3).Value = endingPrice / startingPrice - 1

    Next i

        endTime = Timer
        MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

    End Sub

    Sub formatAllStocksAnalysisTable()
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.Bold = True
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.00%"
    Columns("B").AutoFit
        
    If Cells(4, 3) > 0 Then
        'Color the cell green
        Cells(4, 3).Interior.Color = vbGreen
    ElseIf Cells(4, 3) < 0 Then

        'Color the cell red
        Cells(4, 3).Interior.Color = vbRed

    Else
        'Clear the cell color
        Cells(4, 3).Interior.Color = xlNone

    End If
    
        dataRowStart = 4
        dataRowEnd = 15
        For i = dataRowStart To dataRowEnd

            If Cells(i, 3) > 0 Then

                'Change cell color to green
                Cells(i, 3).Interior.Color = vbGreen

            ElseIf Cells(i, 3) < 0 Then

                'Change cell color to red
                Cells(i, 3).Interior.Color = vbRed
            Else

                'Clear the cell color
                Cells(i, 3).Interior.Color = xlNone
            End If

        Next i


    End Sub
    
    Sub ClearWorksheet()

        Cells.Clear

    End Sub
    
    Sub yearValueAnalysis()
    yearValue = InputBox("What year would you like to run the analysis on?")

    End Sub

Below are two screenshots that show the original code’s run times.

![green_stock_VBA_2017_popout.png](https://github.com/bishopce16/stock-analysis/blob/main/Resources/green_stock_VBA_2017_popout.png)

![green_stock_VBA_2018_popout.png](https://github.com/bishopce16/stock-analysis/blob/main/Resources/green_stock_VBA_2018_popout.png)

Below are two screenshots that show the run time for the refactored code.

![VBA Challenge 2017 popout message](https://github.com/bishopce16/stock-analysis/blob/main/Resources/VBA_Challenge_2017.png)

![VBA Challenge 2018 popout message](https://github.com/bishopce16/stock-analysis/blob/main/Resources/VBA_Challenge_2018.png)

The run times indicate the refactored code runs about 0.2 seconds faster than the original code which is more efficient. 

## Summary Analysis
Besides a faster run time for the macro code, one of the major advantages of refactoring code is that it also helps create a cleaner code. A cleaner code makes the project easier to read and interpret, which can help in debugging, quicker programming, software and design improvements. Another advantage of refactoring VBA script is that you can use as much of the original code as want or need to, however, without a solid grasp of the VBA syntax you could take a working code and potentially make it unusable.
The original code should always be saved, as one of the major disadvantages of refactoring code is that the refactored code could become unusable. 
