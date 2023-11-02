# Cloud attachments

## Microsoft Outlook

When Microsoft Outlook sends the cloud attachment it adds the `Document-Reference` header to message headers. The header contains the URL of the cloud attachment and additional key-value parameters that specify other attributes. The value are encoded with URI encoding scheme to escape special characters and UTF-8 encoding is used where appropriate.

Examples:
```
Document-Reference: https%3a%2f%2f1drv.ms%2fb%2fs!AgaMhbe7wFq8gi8-1E7_mbR71xfI; FileName=EMMC_JESD84-A441.pdf; ProviderType=OneDriveConsumer; Permission=Edit
Document-Reference: https%3a%2f%2f1drv.ms%2fu%2fs!AgaMhbe7wFq8m8VMGyOwHWquKl2ZXA; FileName=test_bu%c5%99t%c3%adk.eml; ProviderType=OneDriveConsumer; Permission=View
```

Defined keys:
- `FileName`
  Name of the attachment file
- `ContentType`
  MIME content type (eg. `application/vnd.openxmlformats-officedocument.wordprocessingml.document`)
- `ProviderType`
  Specifies the type of the cloud provider which affects how other properties are interpreted. Known values are `OneDriveConsumer`, `OneDrivePro`.
- `ProviderUrl`
  For providers that are hosted on premise, such as Sharepoint/OneDrivePro, the URL of the hosted instance (eg. `https://TENANT-my.sharepoint.com/personal/USER_TENANT_onmicrosoft_com`)
- `Permission` is a provider-specific permission
  - `OneDriveConsumer` specifies the values `View` and `Edit`
  - `OneDrivePro` uses `AnonymousEdit` and few others (TODO)
- `Expiration`
  Expiration of the attachment in UTC ticks (number of 100-nanosecond intervals that have elapsed since 12:00:00 midnight, January 1, 0001 in the Gregorian calendar)

An HTML is prepended to the beginning of the message with links to the cloud files in the following shape:
```html
<!--[if lte mso 15 || CheckWebRef]-->
<div id="OwaReferenceAttachments" contenteditable="false">
...
</div>
<!--[endif]-->
```

Full example:
```html
<!--[if lte mso 15 || CheckWebRef]-->
<div id="OwaReferenceAttachments" contenteditable="false">
<table style="margin-bottom: 15px; border-width: 0px 0px 1px 0px; border-color:#C7C7C7; border-style: none none dotted none;">
<tbody>
<tr valign="top">
<td style="padding-bottom:7px;">
<table align="left" style="margin-right: 28px; border-width: 0px; background-color: rgb(255, 255, 255); border-spacing: 0px">
<tbody>
<tr valign="top">
<td style="padding: 0px;">
<div id="OwaReferenceAttachmentDescription" style="margin-left: 3px;">
<p style="white-space: nowrap; margin-bottom: 4px; font-size: 14px; font-weight: normal; font-family: 'Segoe UI', 'Segoe WP', 'Segoe UI WPC', Tahoma, Arial, sans-serif; color: #666666;">
filip.navara@outlook.com has shared a OneDrive file with you. To view it, click the link below.</p>
</div>
</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr valign="top">
<td>
<table valign="top" style="margin-right: 28px; margin-bottom:10px; border-width: 0px; height:20px; background-color: rgb(255, 255, 255); border-spacing: 0px">
<tbody>
<tr valign="top">
<td style="padding: 0px;">
<div style="background-color: rgb(255, 255, 255); height: 20px; width: 20px; max-height: 20px;">
<img height="20" width="20" style="border:0px" src="cid:image00001.png@01DA0DDB.85D26CD0" alt="icon"></div>
</td>
<td>
<div id="OwaReferenceAttachmentFileName2" style="margin: 0px 0px 0px 5px; font-size: 14px; font-family: 'Segoe UI', 'Segoe WP', 'Segoe UI WPC', Tahoma, Arial, sans-serif; color: rgb(0, 114, 198);">
<a href="https://1drv.ms/b/s!AgaMhbe7wFq8gi8-1E7_mbR71xfI" target="_blank" style="text-decoration: none; margin: 0px; font-size: 14px; font-family: 'Segoe UI', 'Segoe WP', 'Segoe UI WPC', Tahoma, Arial, sans-serif; color: rgb(0, 114, 198);">EMMC_JESD84-A441.pdf</a></div>
</td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
</div>
<br>
<div id="OwaReferenceAttachmentsEnd" style="display:none;visibility:hidden;"></div>
<!--[endif]-->
```

The text/plain part of the email contains the converted HTML:
```
filip.navara@outlook.com has shared a OneDrive file with you. To view it, c=
lick the link below.

[icon]
EMMC_JESD84-A441.pdf<https://1drv.ms/b/s!AgaMhbe7wFq8gi8-1E7_mbR71xfI>
```

## IceWarp SmartAttach

IceWarp Mail Server has an option that optionally converts attachments in outgoing messages to cloud links. This is handled on the SMTP server transparently or it can be enforced by adding a message header (TODO: what header).

Received emails get the `X-IceWarp-SmartAttach` message header for each cloud attachment. The header uses a format based on RFC 2047.

TODO: Details, keys (`url`, `name`, `size`, `all`)
TODO: HTML (hidden with `div.icewarp_smartattach { display: none; }`)
