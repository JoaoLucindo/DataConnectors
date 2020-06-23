# Graph API Custom Connector - Power Query SDK

#### Connecting Power BI to Microsoft Graph, a Step-by-Step Guide

Connecting Power BI to Microsoft Graph is a highly requested feature from the  Power BI community. To achieve this, I've delevoped a Custom Connector for Power using the Power Query SDK.

To exemplify the use of this Custom Connector, in this walkthrough I will show you how to get data from Microsoft Planner, however feel free to tie to any Graph API endpoint.


------------

## Step I: Register an application in Azure AD.

In this example, we need to get data from Planner calling the following Graph API endpoint:

`GET /planner/plans/{plan-id}/tasks`

One of the following permissions (Delegated) is required to call this API:

`Group.Read.All, Group.ReadWrite.All`

For more details about this API, please refer to the [documentation](https://docs.microsoft.com/en-us/graph/api/plannerplan-list-tasks?view=graph-rest-1.0&tabs=http "documentation")

With that in mind, lets register our application:

1. Sign in to the [Azure portal](http://portal.azure.com "Azure portal") using either a work or school account or a personal Microsoft account. ()

1. If your account gives you access to more than one tenant, select your account in the top right corner, and set your portal session to the Azure AD tenant that you want.

1. In the left-hand navigation pane, select the Azure Active Directory service, and then select App registrations > New registration.

1. When the Register an application page appears, enter your application's registration information:

	**Name** - Enter a meaningful application name that will be displayed to users of the app.
	**Redirect URL** - Select the Web type, and then enter: `https://oauth.powerbi.com/views/oauthredirect.html` and `https://preview.powerbi.com/views/oauthredirect.html`
1. When finished, select **Register**. 

1. In the left-hand navigation pane, select **Certificates & Secrets** and then select **New Client Secret**. Give a meaninful name and a duration. When done, select Add.

	After saving the client secret, the value of the client secret is displayed. Copy this value because you won't be able to retrieve the key later. Store the key value where you can retrieve it. From now, I'll refer to this key as `%ClientSecret%`

1. In the left-hand navigation pane, select **Overview** and then copy the **Application (client) ID** value. Store the client ID value where you can retrieve it. From now, I'll refer to this key as `%ClientID%`
1.In the left-hand navigation pane, select **API Permission -> +Add Permisson -> Microsoft Graph -> Delegated Permission** and then check `Group.Read.All` permission. When done, select **Add Permission**. After that you need to give the consent, so select **Grant Admin consent for 'your Company name'** 

[![GIF 1](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF1.gif?raw=true "GIF 1")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF1.gif?raw=true "GIF 1")

------------


## Step II: Install the Custom Connector

1. Download this Github Repo: https://github.com/JoaoLucindo/DataConnectors and unzip the file.
1. Go to the folder `C:\..\DataConnectors-master\samples\Planner Connector - Gateway Refresh Support\Planner` and paste your %ClientSecret% and %ClientID% in the file `idsecret.json`.

`  {
    "client_id": "%ClientID%",
    "client_secret": "%ClientSecret%
	} `

When done, save the json file.
1. In the same folder, select all files (except for the file Planner.mez) and add to zip. When done, replace the file extension "**.zip**" for "**.mez**". If you are not able to see the file extension, please check out [this article](https://www.thewindowsclub.com/show-file-extensions-in-windows/ "this article")
1.Check to see if your computer already has a **[Documents]\Power BI Desktop\Custom Connectors** folder. If not, create this folder. After that, copy the recent created **.mez** file to the folder **[Documents]\Power BI Desktop\Custom Connectors**.
1.  You will also need to paste this same **.mez** file on the server on which the Enterprise Gateway is installed into the folder `C:\windows\ServiceProfiles\PBIEgwService\Documents\Power BI Desktop\Custom Connectors`. If your server doesn't have this folder, create the folder. [1]
1. You will also need to adjust your security settings as described in the [custom connector setup documentation.](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-connector-extensibility#data-extension-security "custom connector setup documentation.")

[![GIF2](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF2.gif?raw=true "GIF2")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF2.gif?raw=true "GIF2")


------------

## Step III: Install the Power BI Gateway

1. Follow the steps as decribed in [this doc.](https://docs.microsoft.com/en-us/data-integration/gateway/service-gateway-install "this doc.")
1. Double check [1]:"Step II - 3"

[![GIF 3](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF3.gif?raw=true "GIF 3")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF3.gif?raw=true "GIF 3")

------------

## Step IV: Create the Power BI Report

1. Open your Power BI Desktop.
1. You will need to adjust your security settings as described in the [custom connector setup documentation](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-connector-extensibility#data-extension-security "custom connector setup documentation").
1. Restart your Power BI Desktop
1.In the Home tab of Power BI Desktop, click on **Get Data**.
1.The Get Data window should appear at this point. Navigate to Online Services, then select **PlannerTasks.Content (Beta)** and hit Connect.
1.In the url field paste: `https://graph.microsoft.com/v1.0/planner/plans/{plan-id}/tasks`. Replace the place holder **{plan-id}** by your plan-id that can be found in the planner url. To find the PlanId, open the corresponding Plan in a browser and the last part of the URL contains the PlanId.
In this example, the final url will be like: `https://graph.microsoft.com/v1.0/planner/plans/AOvnbngXFUWDBanG51XCBGUAFOf7/tasks`
[![IMG1](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/IMG1.png?raw=true "IMG1")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/IMG1.png?raw=true "IMG1")

1. When done, hit **OK**, signin with your organization account and finaly hit **Connect**
[![GIF421](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF421.gif?raw=true "GIF421")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF421.gif?raw=true "GIF421")

1. Now you can transform and modify your data the way you want, and finally create your report.
1. When finish, hit **Publish** to publish your report to the Power BI Service. For more details, please refer to [this doc.](https://docs.microsoft.com/en-us/power-bi/create-reports/desktop-upload-desktop-files "this doc.")

[![GIF422](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF422.gif?raw=true "GIF422")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF422.gif?raw=true "GIF422")
------------

## Step V: Configure Schedule Refresh

1. Go to [https://admin.powerplatform.microsoft.com/ext/DataGateways](https://admin.powerplatform.microsoft.com/ext/DataGateways "https://admin.powerplatform.microsoft.com/ext/DataGateways")
1. Select your Data Gateway, and then click on **More options (...)**
1. Select **Settings**
1. In the bottom right hand corner, check both the box labeled **Allow user's cloud datasources to refresh through this gateway cluster** and **Allow user's custom data connectors to refresh through this gateway cluster.**
[![GIF5](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF5.gif?raw=true "GIF5")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF5.gif?raw=true "GIF5")
1. Go to [https://app.powerbi.com/groups/me/gateways](https://app.powerbi.com/groups/me/gateways "https://app.powerbi.com/groups/me/gateways")
1. Select your Gateway and then hit **Add data sources to use the gateway**
1. Give a name for your new DataSource, Select **GraphAPIConnector** for Data Source Type, and then paste the same url that we've created in the **Step IV - 6**.
1. Select **Edit Credentials**, Signin and then click **Add**
[![GIF6](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF6.gif?raw=true "GIF6")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF6.gif?raw=true "GIF6")
1. Go to the report that we just published to Power BI Service
1. In you bottom left-hand corner, In the navigation pane, under Datasets, select More options (...)
1. Make sure you're configuring the dataset with the same name as the report you just published.
1. Select **Schedule Refresh**.
1. Open the **Gateway connection** blade and change the **Use a data gateway** toggle switch to **On**
1. Finaly, maps the data source to the data source that you just added to the gateway and hit **Apply**
1. Now you can turn on the **Schedule Refresh** and configure the refresh frequency as you want and keep your report updated.
[![GIF7](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF7.gif?raw=true "GIF7")](https://github.com/JoaoLucindo/DataConnectors/blob/master/GIFs/GIF7.gif?raw=true "GIF7")
------------

# References

[1] [Use custom data connectors with the on-premises data gateway](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-custom-connectors#considerations-and-limitations "Use custom data connectors with the on-premises data gateway")

[2] [Considerations and limitations](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-custom-connectors#considerations-and-limitations "Considerations and limitations")

[3] [Create custom Power Query connectors](https://docs.microsoft.com/en-us/power-query/startingtodevelopcustomconnectors "Create custom Power Query connectors")

[4] [Getting Started with Data Connectors](https://github.com/Microsoft/DataConnectors "Getting Started with Data Connectors")



------------

# Troubleshotting

[Troubleshoot gateways - Power BI](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-onprem-tshoot "Troubleshoot gateways - Power BI")
