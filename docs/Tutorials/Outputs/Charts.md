# Charts

This page details the available output actions available to Charts.

## Out

To create a new chart, usually appended beneath the sending element, you can use [`Out-PodeWebChart`](../../../Functions/Outputs/Out-PodeWebChart).  The `-Data` supplied can either raw or from [`ConvertTo-PodeWebChartData`](../../../Functions/Outputs/ConvertTo-PodeWebChartData):

```powershell
New-PodeWebContainer -NoBackground -Content @(
    New-PodeWebButton -Name 'Show Processes' -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 15 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU |
            Out-PodeWebChart -Type Line
    }
)
```

## Update

To update the data points of a chart on the page, you can use [`Update-PodeWebChart`](../../../Functions/Outputs/Update-PodeWebChart). The `-Data` supplied can either raw or from [`ConvertTo-PodeWebChartData`](../../../Functions/Outputs/ConvertTo-PodeWebChartData):

```powershell
New-PodeWebContainer -NoBackground -Content @(
    New-PodeWebButton -Name 'Update Processes' -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 15 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU |
            Update-PodeWebChart -Name 'Processes'
    }

    New-PodeWebChart -Name 'Processes' -Type Line -NoRefresh -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 15 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU
    }
)
```

## ConvertTo

The [`ConvertTo-PodeWebChartData`](../../../Functions/Outputs/ConvertTo-PodeWebChartData) simplifies using the raw format, by letting you convert data at the end of a pipeline. The function takes a `-LabelProperty` which is the name of a property in the input that should be used for the X-axis, and then a `-DatasetProperty` with is property names for Y-axis values.

For example, let's say we want to display the top 10 processes using the most CPU. We want to display the process name (x-axis), and its CPU and Memory usage (y-axis):

```powershell
New-PodeWebContainer -Content @(
    New-PodeWebChart -Name 'Top Processes' -Type Bar -AutoRefresh -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 10 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU, Handles
    }
)
```

## Sync

To force a chart to refresh its data you can use [`Sync-PodeWebChart`](../../../Functions/Outputs/Sync-PodeWebChart):

```powershell
New-PodeWebContainer -NoBackground -Content @(
    New-PodeWebButton -Name 'Refresh Processes' -ScriptBlock {
        Sync-PodeWebChart -Name 'Processes'
    }

    New-PodeWebChart -Name 'Processes' -Type Line -NoRefresh -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 15 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU
    }
)
```

## Clear

To clear a chart's data you can use [`Clear-PodeWebChart`](../../../Functions/Outputs/Clear-PodeWebChart):

```powershell
New-PodeWebContainer -NoBackground -Content @(
    New-PodeWebButton -Name 'Clear Processes' -ScriptBlock {
        Clear-PodeWebChart -Name 'Processes'
    }

    New-PodeWebChart -Name 'Processes' -Type Line -NoRefresh -ScriptBlock {
        Get-Process |
            Sort-Object -Property CPU -Descending |
            Select-Object -First 15 |
            ConvertTo-PodeWebChartData -LabelProperty ProcessName -DatasetProperty CPU
    }
)
```
