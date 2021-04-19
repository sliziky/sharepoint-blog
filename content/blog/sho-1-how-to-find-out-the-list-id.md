---
path: how-to-find-out-id-of-the-list
date: 2021-04-19T20:23:49.451Z
title: "SHO #1 - How to find out the list Id"
description: Three easy ways to find out the Id
---
Each SharePoint list has a unique Guid (Id) - there may be a situation when you are going to need it. For example when you want to perform an operation on that list.

### Solution #1 - Sharepoint:

1. Open a specific List
2. Press a **gear button** in the upper-right corner
3. Open **List settings**
4. Now you can see your URL is in the form **"https://...?List=%7B...%7D"**
5. The string between **%7B** and **%7D** is your ID

The reason why there also are these weird symbols is simple - the URL is encoded, but if you decode it you are going to find out that **%7B = '{'** and **%7D = '}'**

### **Solution #2 - Sharepoint API:**

1. Delete all parts of your URL until you get it in the form of `https://<your_sharepoint>/sites/<name_of_the_site>`
2. Append `/_api/lists/getByTitle('<name_of_list>')` to the end of your edited URL
3. You are going to see an XML scheme (if it is messy try using [this](https://chrome.google.com/webstore/detail/sp-rest-json/kcdolhjbipnfgefpjaopfbjbannphidh)) where you can easily find the Id under the field **Id**



### **Solution #3 - SPFX:**

This solution is here just to show the possibility of using SPFX for this - it is not a good approach if finding Id of only one list.

```typescript
// inside your webpart code
import { sp } from "@pnp/sp";
import { IListInfo } from "@pnp/sp/presets/all";

// configure SP to the correct SHP site
const configuredSp = sp.configure(null, urlOfYourSite);

// lists variable is the array of all the lists from the web
// method returns the array of IListInfo objects - what this object contains is shown below
const lists : IListInfo[] = await configuredSp.web.lists();

// types.d.ts
interface IListInfo {
    // other stuff
    Id: string;
    // other stuff
    Title: string;
    // other stuff
}
```