# Tools - Windows Performance Toolkit for HDD Write Monitoring

## Disclaimer

This guide is a personal project and not a peer-reviewed publication or sponsored document. It is provided “as is,” without any warranties—express or implied—including, but not limited to, accuracy, completeness, reliability, or suitability for any purpose. The author(s) shall not be held liable for any errors, omissions, delays, or damages arising from the use or display of this information.

All opinions expressed are solely those of the author(s) and do not necessarily represent those of any organization, employer, or other entity. Any assumptions or conclusions presented are subject to revision or rethinking at any time.

Use of this information, code, or scripts provided is at your own risk. Readers are encouraged to independently verify facts. This content does not constitute professional advice, and no client or advisory relationship is formed through its use.

## Description

This repository demonstrates how to use microsoft tools to see what processes are writing to the hard drive.

- [Resource Monitor (resmon.exe)][#resource-monitor-requirements]
- [Windows Performance Toolkit](#windows-performance-toolkit-requirements)
- [Process Explorer](#process-explorer-requirements)

## Resource Monitor Requirements

- Windows 10 or later

## Step 1:

- Run resmon.exe from the Windows start menu

## Step 2:

- Change to the Disk Tab, and observe the disk activity by process and file name.

![resmon](./docs/images/resmon.png)

## Step 3:

- Identify the files with the highest WriteB/S

## Windows Performance Toolkit Requirements

- Windows 10 or later
- Admin privileges
- Windows Performance Toolkit

## Step 1: Install Windows Performance Toolkit

1. Download the **Windows ADK**:
   [https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install)

2. During setup, select only the Windows Performance Toolkit

![WPT](./docs/images/install-wpt.png)

This installs:

- `WPR.exe` (recorder)
- `WPA.exe` (analyzer)

## Step 2: Start a Disk I/O Trace

1. Open **Command Prompt as Administrator**.
2. Start the trace using:

   ```sh
   WPR -start diskio -filemode
   ```

   This will begin recording detailed disk write activity to disk.

## Step 3: Reproduce the Disk Activity

- Let your computer run normally, or perform the action that causes high disk usage.
- Record for a few minutes.

## Step 4: Stop and Save the Trace

When you're ready, stop the trace and save the results:

```sh
WPR -stop highdisk.etl
```

This creates a file named `highdisk.etl` that contains performance trace data.

## Step 5: Analyze the Trace with WPA

1. Launch **Windows Performance Analyzer**:

   ```sh
   wpa.exe
   ```

2. Open the `highdisk.etl` file.

   ![WPT](./docs/images/open-etl.png)

3. In the left-hand Graph Explorer pane, add the following graph:

   - `Activity by IO Type, Process`

   ![WPT](./docs/images/add-graph.png)

4. Drill down by:

   - **Write**
   - **Process**

   ![WPT](./docs/images/read-graph.png)

## Clean Up

You can delete `.etl` files after analysis. They may be large depending on trace duration.

## Uninstall

Windows Performance Analyzer can be uninstalled using the standard "Add remove programs" and uninstalling the "Windows Assessment and Deployment Kit" (may also be under Windows Performance Toolkit).

## Process Explorer Requirements

- Windows 10 or later
- Admin privileges
- Process Explorer

## Step 1: Install Process Explorer

1. Download the **Process Explorer zip file**:

   [https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer#download](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer#download)

2. Extract the zip file to a suitable location and run procexp.exe

   ![Extract and Run](./docs/images/pex-extract-run.png)

## Step 2: Setup Process Explorer

1. Right click the "Description" column, then "Select Columns..."

   ![Select Columns](./docs/images/pex-select-columns.png)

2. Ensure that Process Disk > Write Bytes is selected

   ![Write Bytes](./docs/images/pex-select-write-bytes.png)

## Step 3: Analyze using the view.

Use the view to see the processes writing to the hard drive

![Analyze Columns](./docs/images/pex-column.png)
