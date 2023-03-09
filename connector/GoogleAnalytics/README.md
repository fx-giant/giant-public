# Google Analytics Connector Guide

This documentation provides guidance to obtain Google Analytics Account ID, and to setup Google Analytics OAuth credential in order to connect to Google Analytics via GIANT as a connector.

# Prerequisite
- GIANT account with permission create connection source
- Google account with
    - Google Analytics Account
    - Permission to create credential - client ID

# 1. Obtaining Google Analytics Account ID
## Obtain Google Analytics Account ID 
1. Login to Google Analytics Platform. 
2. Expand the **Accounts** breakdown on the top left panel. 
3. Navigate to your preferred Account and obtain the **Account ID** under **Analytics Accounts** tab.

# 2. Preparation for Google Analytics OAuth Credential
## Create Google Credential - OAuth Client ID
1. Login to Google Cloud Platform.
2. Navigate to **Credentials** under **APIs & Services**
3. Click the **Create credentials** button and choose **OAuth client ID**.
4. Under **Application type**, select **Web application**.
5. Enter name for this client ID. Example GIANT Google Analytics.
6. Enter GIANT URL into **Authorised JavaScript origins**. 
7. Add GIANT URL and [**Google OAuth 2.0 Playground**](https://developers.google.com/oauthplayground) into **Authorised redirect URIs**. ![create-client-id](images/create-oauth-client-id.png)
8. Click **Create** button. Upon success create, the new client ID is show at the Credential page.
9. Navigate into the new OAuth Client ID to get the **Client ID** & **Client secret**. ![client-id-credential](images/client-id-web-app.png)

## Generate refresh token
1. Go to [**Google OAuth 2.0 Playground**](https://developers.google.com/oauthplayground/).
2. Click **OAuth 2.0 Configuration** icon button at the top right. OAuth 2.0 configuration box will be popout.
3. Select `Select Account Screen` at **Force prompt**.
4. Check the **Use your own OAuth credentials**. 
5. Enter the **OAuth Client ID** & **OAuth Client Secret**.
![oauth-configuration](images/playground-oauth-configuration.png)
6. Under **Step 1 Select & authorize APIs**, find **Analytics Reporting API v4**. Tick the following. ![oauth-step1](images/playground-authorize-api-new.png)
7. Navigate to **Google Analytics API v3**. Tick the following. ![oauth-step1](images/playground-authorize-api-2-new.png)
8. Click the **Authorize APIs** button to proceed. It will redirect to authorization page, continue to allow the Google Analytics permission for GIANT. Once complete, you will be redirect back to the **OAuth 2.0 Playground** page.
9. Notice the page will show you Step 2 with **Authorization code** available. Click the **Exchange authorization code for tokens** button. 
10. **Refresh token** and **Access token** are ready. 
![exchange-token](images/playground-exchange-tokens.png)

# 3. Enable Google Analytics API and Google Analytics Reporting API
1. Go to [**Google Cloud Platform APIs & Services Page**](https://console.cloud.google.com/apis/dashboard).
2. Click **ENABLE APIS AND SERVICES** button at the top left. ![enable apis](images/enable-ga-apis.PNG)
3. You will be redirected to Google API Library page. Search **Google Analytics** in the search bar and these API options will be displayed on your page. ![search apis](images/search-ga-apis.PNG)
4. Click on **Google Analytics Reporting API** and click on the **ENABLE** button to enable the Analytics Reporting API. ![enable reporting](images/enable-reporting-api.PNG)
5. Next, click on **Google Analytics API** and click on the **ENABLE** button to enable the Analytics API. ![enable analytics](images/enable-analytics-api.PNG)
6. You have successfully enabled the Google Analytics API needed to create and use Google Analytics connection source in GIANT. 

# 4. Create Google Analytics connection source in GIANT
1. Go to GIANT new connection source add page. Select Google Analytics.
![New Connection Source Page](images/giant-new-connection-source.jpeg)
2. Fill in all the information obtain from above. ![Create Giant Google Analytics](images/google-analytics-connector.jpeg)
3. After filling in all details, click **Connect** to connect to the server. 
4. The system will lead you to the Properties & Apps selection page. ![Properties & Apps](images/property_selection_pane.jpeg)
5. Select the Property you would like to connect to and click **Next**.
6. The system will lead you to the View(s) selection page. ![View](images/view_selection-pane.jpeg)
7. Select the View you would like to connect to and click **Next**.
8. The system will lead you to the Dataset(s) selection page. ![Dataset](images/dataset_selection_pane.jpeg)
9. Select the dataset you would like to connect to and click **Save**.
10. You have successfully created Google Analytics connection source in GIANT. 
