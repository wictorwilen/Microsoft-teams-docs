# Use the Seller Dashboard to submit your Microsoft Teams app

For a general overview of the Microsoft Teams app submission flow, see [Publish your Microsoft Teams app to the Office Store](submission.md).

In the Seller Dashboard:

1. Enter the Office dashboard by choosing **Continue** under the Office heading.

   ![Office Seller Dashboard entry point](images/submission/SellerDashboardOfficeEntry.PNG)

2. Choose **Add a new app** to initiate the submission process.

   ![Office Seller Dashboard Add an app button](images/submission/SellerDashboardAddApp.PNG)

3. Select the **Teams app** option.

   ![Office Seller Dashboard Overview](images/submission/SDAppType.PNG)

4. Enter the overview information for your app.

   ![Office Seller Dashboard app overview](images/submission/SDOverviewCrop.PNG)

   * **App package** – Upload your properly formatted 1.0 manifest. Note that the manifest information prepopulates many of the following fields.
   * **Submission title** – The name of your app.
   * **Version** – Autopopulated from the manifest.
   * **Release date** – By default, today's date. This means that your app is eligible to be published as soon as it passes validation. If you want to delay publication, specify the date. Please note: Validation times vary, and there is no guarantee the validation process will be done by the specified date.
   * **Category** – You can select up to three categories in which to appear in the Office Store. Please select ones that best match your experience.
   * **Testing notes** – Please ensure you provide enough information for our validation team to successfully load and test your experience. This includes providing sign-in instructions and test accounts, as well as any other notes that might assist in the review of your product.
   * **My app calls, support, contains, or uses cryptography or encryption** – This checkbox helps to ensure taht your product is not using cryptography in a way that would prohibit distribution through the Office Store. See [Export restrictions on cryptography](https://docs.microsoft.com/en-us/windows/uwp/security/export-restrictions-on-cryptography) for a detailed explanation.
   * **App logo** – Use your full-color 96×96 icon.
   * **Support document link** – The URL for support content for your app.
   * **Privacy document link** – Use the URL that appears in `privacyUrl` in your manifest.
   * **Video link** – Optional; the URL of a video of your app. This video appears on your product listing page in the Office Store.
   * **End User License Agreement** – If you need a custom EULA, upload it here.

   Choose **Next** when all required information has been filled in.

4. Enter the detail information for your app.

   ![Office Seller Dashboard app details](images/submission/SDDetails.PNG)

   * **Language** – Select the language your app supports. (Currently there is no way to provide an app with multiple languages. You may submit your app in one language only.)
   * **App name** – This field would specify your localized name, but for now this should be the same as your app name in the manifest.
   * **Short description** – Use the short description from the manifest.
   * **Long description** – This description is your opportunity to present your experience to end users; it is shown in the app discovery flow in Microsoft Teams. Although you can use the long description from the manifest, feel free to add more content and take advantage of the formatting options.
   * **Screenshots** – You must include at least one 1366×768 screenshot. Your screenshot *must* accurately reflect the experience of your Microsoft Teams app.

   Choose **Next** when all required information has been filled in.

5. Opt out of any regions you do not want to be distributed in.

   ![Office Seller Dashboard region block](images/submission/SDBlockRegions.PNG)

   If you do not want to distribute your app in all regions where the Office Store is available, select the regions in which you do *not* want your app to appear.

   ![Office Seller Dashboard available regions](images/submission/SDRegions.PNG)

6. The Lead Management page is currently not supported in Microsoft Teams. Choose **Next**.

7. Currently, Microsoft Teams allows only free apps, so the only option is **This app is free**.

   ![Office Seller Dashboard pricing and submit](images/submission/SDPricing.PNG)

   If you have filled everything in correctly, choose **Submit for approval**. To return later, choose **Save as draft**.

>For more information on the validation process, see [Microsoft Teams app approval process](submission.md#microsoft-teams-app-approval-process).
