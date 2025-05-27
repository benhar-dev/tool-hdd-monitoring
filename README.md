# Tools - Windows Performance Toolkit for HDD Write Monitoring

## Disclaimer

This is a personal guide not a peer reviewed journal or a sponsored publication. We make
no representations as to accuracy, completeness, correctness, suitability, or validity of any
information and will not be liable for any errors, omissions, or delays in this information or any
losses injuries, or damages arising from its display or use. All information is provided on an as
is basis. It is the reader’s responsibility to verify their own facts.

The views and opinions expressed in this guide are those of the authors and do not
necessarily reflect the official policy or position of any other agency, organization, employer or
company. Assumptions made in the analysis are not reflective of the position of any entity
other than the author(s) and, since we are critically thinking human beings, these views are
always subject to change, revision, and rethinking at any time. Please do not hold us to them
in perpetuity.

## Description

This repository demonstrates how to use Windows Performance Toolkit to see what processes are writing to the hard drive.

## Requirements

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
