# `arduino/report-size-trends` action

This action records the memory usage of the [Arduino](https://www.arduino.cc/) sketch specified to the [`arduino/compile-examples` action](https://github.com/arduino/compile-sketches)'s [`size-report-sketch` input](https://github.com/arduino/compile-sketches#size-report-sketch) to a [Google Sheets](https://www.google.com/sheets/about/) spreadsheet.

## Inputs

### `sketches-report-path`

Path that contains the JSON formatted sketch data report, as specified to the `arduino/compile-examples` action's [sketches-report-path input](https://github.com/arduino/compile-sketches#sketches-report-path).

**Default**: `"size-deltas-reports"`

### `google-key-file`

**Required** Contents of the Google key file used to update the size trends report Google Sheets spreadsheet. This should be defined using a [GitHub encrypted secret](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).

#### Create Google API credentials

1. Open https://console.developers.google.com/project
1. Click the "Create Project" button.
1. In the "Project name" field, enter the name you want for your project.
1. You don't need to select anything from the "Location" menu.
1. Click the "Create" button.
1. Wait for project creation to finish.
1. Click the button with the three horizontal lines at the top left corner of the window.
1. Hover the mouse pointer over "APIs & Services".
1. Click "Library".
1. Make sure the name of the project you created is selected from the dropdown menu at the top of the window.
1. Click 'Google Sheets API".
1. Click the "Enable" button.
1. Click the "Create Credentials" button.
1. From the "Which API are you using?" menu, select "Google Sheets API".
1. From the "Where will you be calling the API from?" menu, select "Other non-UI".
1. From the "What data will you be accessing?" options, select "Application data".
1. From the "Are you planning to use this API with App Engine or Compute Engine?" options, select "No, I’m not using them".
1. Click the "What credentials do I need?" button.
1. In the "Service account name" field, enter the name you want to use for the service account.
1. From the "Role" menu, select "Project > Editor".
1. From the "Key type" options, select "JSON".
1. Click the "Continue" button. The .json file containing your private key will be downloaded. Save this somewhere safe.

#### Create key secret

1. Open the downloaded Google API key file you created by following the "Create Google API credentials" instructions above.
1. Copy the entire contents of the file to the clipboard.
1. Open the GitHub page of the repository you are configuring the GitHub Actions workflow for.
1. Click the "Settings" tab.
1. From the menu on the left side of the window, click "Secrets".
1. Click the "New secret" button.
1. In the "Name" field, enter the variable name you want to use for your secret. This secret is what you will use in the `arduino/report-size-trends` action's `google-key-file` input in your GitHub Actions workflow configuration file. For example, if you named the secret `GOOGLE_KEY_FILE`, the input would look like `google-key-file: ${{ secrets.GOOGLE_KEY_FILE }}`.
1. In the "Value" field, paste the contents of the key file.
1. Click the "Add secret" button.

#### Configure Google Sheets spreadsheet access

1. Open the downloaded Google API key file you created by following the "Create Google API credentials" instructions above.
1. Copy the email address shown in the `client_email` field.
1. Open Google Sheets: https://docs.google.com/spreadsheets
1. Under "Start a new spreadsheet", click "Blank".
1. Click the "Share" button at the top right corner of the window.
1. If you haven't already, give your spreadsheet a name.
1. Paste the `client_email` email address into the "Enter names or email addresses..." field.
1. Click the email address in the auto-complete dropdown that appears under the email "Enter names or email addresses..." field.
1. From the permissions menu to the right of the field containing the email, select "Editor"
1. Uncheck the box next to "Notify people".
1. Click the "Share" button.

### `spreadsheet-id`

**Required** The ID of the Google Sheets spreadsheet to write the memory usage trends data to. The URL of your spreadsheet will look something like:
```
https://docs.google.com/spreadsheets/d/15WOp3vp-6AnTnWlNWaNWNl61Fe_j8UJhIKE0rVdV-7U/edit#gid=0
```
In this example, the spreadsheet ID is `15WOp3vp-6AnTnWlNWaNWNl61Fe_j8UJhIKE0rVdV-7U`.

### `sheet-name`

The sheet name in the Google Sheets spreadsheet used for the memory usage trends report.

**Default**: `"Sheet1"`

## Example usage

```yaml
- uses: arduino/compile-sketches@main
# Publish size trends report on each push to the main branch
- if: github.event_name == 'push' && github.ref == 'refs/heads/main'
  uses: arduino/report-size-trends@main
  with:
    google-key-file: ${{ secrets.GOOGLE_KEY_FILE }}
    spreadsheet-id: 15WOp3vp-6AnTnWlNWaNWNl61Fe_j8UJhIKE0rVdV-7U
```
