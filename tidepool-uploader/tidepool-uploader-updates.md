## Tidepool Uploader Updates.md

> Visit https://tidepool.org/uploader to download the latest version of the Tidepool Uploader.  

> If you already have the Tidepool Uploader installed on your computer, it will automatically update to the latest version.
### 2.5.11 (Released 5/16/2018)
Tidepool now supports Medtronic Veo (554/754) insulin pumps. Yay!  
[Trello Details](https://trello.com/c/RU2SjKA1/7-uploader-support-medtronic-veo-554-754-pump)

Bayer was acquired by Ascencia, which means all those Contour meters needed some new branding. We’ve gone with Ascencia (Bayer) to minimize confusion.  
[Trello Details](https://trello.com/c/mt2xb3qV/6-update-bayer-listing-to-ascencia-bayer)

Our friends at Medtronic removed the ability to export your data as a CSV file. We relied on that functionality to import your CareLink data into Tidepool. As a result, we’ve removed the “Upload from CareLink option in the Tidepool Uploader” (Don’t worry, you can still upload directly to Tidepool with your Contour Next Link meter.)  
[Trello Details](https://trello.com/c/pz3ltLai/4-remove-upload-from-carelink-option)

Previously, if two consecutive square wave calculator boluses were given on a Medtronic 530G, the Tidepool Uploader interpreted the data as a single Dual Wave bolus with the extended portion of the bolus being the insulin total from the first bolus, which is listed as an override of the total. Also, if the last bolus on the 530G was a square wave bolus, data uploads failed. It’s hip to be square, but not that square - we’ve fixed both those issues.  
[Trello Details](https://trello.com/c/izgIetxG/5-2-consecutive-square-calculator-boluses-are-merged-into-a-single-bolus-when-uploaded-from-medtronic-530g)

Finally, if an account was set up to upload devices on one operating system, you’d see “Unknown Device” on an operating system that does not support those devices. Now, we will remove that device from your upload options so it doesn’t look like you’re trying to upload mystery data.  
[Trello Details](https://trello.com/c/6XAazELZ/8-unknown-devices-appearing-in-uploader)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.11)

### 2.5.9 (Released 4/26/2018)
We change some of the text for our Time Check pop up to make it clear that you should triple-check the time on the device you’re trying to upload.  
[Trello Details](https://trello.com/c/ERwBfwRa/19-update-device-time-check-copy)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.9)

### 2.5.8 (Released 4/26/2018)
Dependency updates aren’t always the most exciting thing, but we want to make sure you’re getting the latest and greatest software possible, so here we are.  
[Trello Details](https://trello.com/c/LLQU6QaX/17-new-card-uploader-dependency-updates)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.8)

### 2.5.7 (Released 4/23/2018)
Remember when we said we fixed that bug that prevented all your Libre data from uploading. This time we really, really fixed it. (To be fair, the FreeStyle Libre hasn’t been out that long, so it took a little time for larger datasets to come our way and push the boundaries of our code.)  
[Trello Details](https://trello.com/c/UUfpJcP2/10-reading-all-the-available-data-from-a-libre-device)

For our friends with a Medtronic pump using more than 6 or 7 insulin sensitivities or carb ratios, your uploads used to fail. We fixed that too.  
[Trello Details](https://trello.com/c/MyWefnN2/11-upload-fails-when-a-large-number-of-insulin-sensitivities-or-carb-ratios-are-specified)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.7)

### 2.5.5 (Released 4/10/2018)
Be honest, how diligent are you at keeping your devices on time when you travel? The Tidepool Uploader will now warn you if it detects a difference between the time zone selected and the time indicated on your device.  
[Trello Details](https://trello.com/c/1eAw2Fpc/2-uploader-timezones-as-a-user-i-want-the-uploader-to-warn-me-of-any-difference-between-my-device-time-and-computer-time-or-select)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.5)

### 2.5.4 (Released 3/27/2018)
We found a bug that prevented every last data point on your FreeStyle Libre from uploading. That won’t be a problem anymore.  
[Trello Details](https://trello.com/c/5KPT5Iim/20-libre-uploading-with-gaps-in-data)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.4)

### 2.5.3 (Released 3/16/2018)
We’ve made improvements to our Uploader error-logging service, Rollbar to catch more hiccups and bummers that you throw our way. This will go a long way to helping us identify areas we can improve our software.  
[Trello Details](https://trello.com/c/144Btqt7/13-uploader-improve-rollbar-logging)

Omnipod device settings were hell-bent on displaying as mg/dL even when uploaded from a pump set to mmol/L. We’d like to formally apologize to our friends who prefer blood glucose concentrations measured by molarity instead of weight - this has been fixed.  
[Trello Details](https://trello.com/c/KVEO8OTR/12-omnipod-showing-mg-dl-in-device-settings-even-after-mmol-l-selected-in-user-profile)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.3)

### 2.5.1 (Released 3/12/2018)
We gave upload times for Medtronic and Tandem pumps a serious speed boost. Prior to this update, we would pull all data off compatible Medtronic and Tandem pumps and sort out duplicate data later. Now, initial uploads of compatible Tandem and Medtronic pumps will show “We've improved how devices upload. This upload will take longer than usual, but your future uploads will be much, much faster.” After that, we will only upload new data. The result? A bolus from the Speed Force and much faster uploads in the future.  
Trello Details  
[Part 1](https://trello.com/c/gwB5pykC/3-uploader-delta-upload-ui), [Part 2](https://trello.com/c/5i0sM0wx/4-m-delta-uploads-3-of-4-medtronic-5-series), [Part 3](https://trello.com/c/ieSwFQq5/5-m-delta-uploads-4-of-4-tandem)

[GitHub Details](https://github.com/tidepool-org/chrome-uploader/releases/tag/v2.5.1)