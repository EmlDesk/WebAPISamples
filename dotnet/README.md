Emldesk API Wrapper
=============

This library allows you to easily interact with the Emldesk API in .NET.

Installation
------------

This library is distributed via [NuGet](http://nuget.org/packages/emldesk).

    PM> Install-Package emldesk

Usage
------------

Using the library is as easy as creating an `Api` object:

```csharp
var emldesk = Emldesk.Api.Create("token");
```

A timeout can be set when instantiating an API or HttpClient object, ex:
```csharp

var api = Emldesk.Api.Create("my_secret_toke", allowSelfSignedCerts: true, timeout: 1200)
```
or 
```charp
var httpClient = new HttpClient(baseUrl, accountKey + @"\" + username, password, allowSelfSignedCerts, proxy, timeout);
```
Default timeout is 30 seconds. It may be necessary to increase this for large imports/exports

From there you can navigate the hierarchy using intellisense:

```csharp
var lists = emldesk.Lists.Filtered.Post(new ListFilter { LargerThan = 500 });
lists.ForEach(x => Debug.WriteLine("{0}: {1}", x.Name, x.TotalActiveRecipients));
```

See the [documentation](http://services.emldesk.net/) under the *Examples* section for specific examples.

Most calls to the API accept an optional account id argument. The wrapper defaults to the account id of the authentication user so you do not need to set this.

The library structure closely follows url structure of the API so the [documentation](http://services.emldesk.net/) is in parity with the library. The properties in the library first match up to the url segments (Url parameters other than account id are prefixed with "By") and finally the http verb. For example the endpoint url  `GET:/reports/campaigns/{AccountId}?enddate={enddate}&startdate={startdate}` would correspond to `emldesk.Reports.Campaigns.Get(startdate: ..., enddate: ...)`. Where possible comments in the online documentation are included in the library so you can see them in Intellisense.

If you are accessing the API behind a proxy server you can specify a proxy server as follows:

```csharp
var emldesk = Emldesk.Api.Create("token", proxy: new WebProxy("http://webproxy:80/"));
```
