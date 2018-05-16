## Starter site

Ecwid API features several endpoints to control the presentation of Starter site. Learn more about the [Ecwid Starter site](https://support.ecwid.com/hc/en-us/articles/207100069-Starter-site)

<aside class="note">Access scope required: <strong>update_store_profile</strong></aside>

### Get starter site content details

Get starter site content details of an Ecwid store.

#### Request

> Request example

```http
GET /api/v3/4870020/startersite?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`GET https://app.ecwid.com/api/v3/{storeId}/startersite?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

<aside class="notice">
Parameters in bold are mandatory
</aside>

#### Response

> Response example (JSON)

```json
{
  "coverImageUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/desktop.jpg",
  "coverImageThumbnail": "data:image/jpeg;base64,/9j/4QAyRXhpZgAASUkqAAgAAAABAJiCAgAOAAAAGgAAAAAAAABnLXN0b2Nrc3R1ZGlvAAAA/+wAEUR1Y2t5AAEABAAAAB4AAP/hBLtodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIgeG1sbnM6cGhvdG9zaG9wPSJodHRwOi8vbnMuYWRvYmUuY29tL3Bob3Rvc2hvcC8xLjAvIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkE0OTJFNUU4Mzk2NjExRTY5ODhCRUE4MDE2OUI1NEUzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkE0OTJFNUU3Mzk2NjExRTY5ODhCRUE4MDE2OUI1NEUzIiB4bXA6Q3JlYXRvclRvb2w9IkFkb2JlIFBob3Rvc2hvcCBMaWdodHJvb20gNS4wIChXaW5kb3dzKSIgcGhvdG9zaG9wOkF1dGhvcnNQb3NpdGlvbj0iQ29udHJpYnV0b3IiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0iQUM1RjYxQTNCNDQ2MEI4NEE0RUZDNTExQzAxQUI4NUQi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",
  "coverImageMobileUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/mobile.jpg",
  "coverImageMobileThumbnail": "data:image/jpeg;base64,/9j/4QAyRXhpZgAASUkqAAgAAAABAJiCAgAOAAAAGgAAAAAAAABnLXN0b2Nrc3R1ZGlvAAAA/+wAEUR1Y2t5AAEABAAAAB4AAP/hBLtodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIgeG1sbnM6cGhvdG9zaG9wPSJodHRwOi8vbnMuYWRvYmUuY29tL3Bob3Rvc2hvcC8xLjAvIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkQwQTMyQkM3Mzk2NjExRTY5ODhCRUE4MDE2OUI1NEUzIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkQwQTMyQkM2Mzk2NjExRTY5ODhCRUE4MDE2OUI1NEUzIiB4bXA6Q3JlYXRvclRvb2w9IkFkb2JlIFBob3Rvc2hvcCBMaWdodHJvb20gNS4wIChXaW5kb3dzKSIgcGhvdG9zaG9wOkF1dGhvcnNQb3NpdGlvbj0iQ29udHJpYnV0b3IiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0iQUM1RjYxQTNCNDQ2MEI4NEE0RUZDNTExQzAxQUI4NUQiIHN0UmVmOmRvY3VtZW50SUQ9IkFDNUY2MUEzQjQ0NjBCODRBNEVGQzUxMUMwMUFCODVEIi8+IDxkYzpyaWdodHM+IDxyZGY6QWx0PiA8cmRmOmxpIHhtbDpsYW5nPSJ4LWRlZmF1bHQiPmctc3RvY2tzdHVkaW88L3JkZjpsaT4gPC9yZGY6QWx0PiA8L2RjOnJpZ2h0cz4gPGRjOmNyZWF0b3I+IDxyZGY6U2VxPiA8cmRmOmxpPmctc3RvY2tzdHVkaW88L3JkZjpsaT4gPC9yZGY6U2VxPiA8L2RjOmNyZWF0b3I+IDxkYzp0aXRsZT4gPHJkZjpBbHQ+IDxyZGY6bGkgeG1sOmxhbmc9IngtZGVmYXVsdCI+NDc4MjE0OTE2PC9yZGY6bGk+IDwvcmRmOkFsdD4gPC9kYzp0aXRsZT4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7/7QBaUGhvdG9zaG9wIDMuMAA4QklNBAQAAAAAACEcAVoAAxslRxwCAAACAAIcAnQADWctc3RvY2tzdHVkaW8AOEJJTQQlAAAAAAAQabWF9sA1wBHTSIwEgOM32f/uAA5BZG9iZQBkwAAAAAH/2wCEABALCwsMCxAMDBAXDw0PFxsUEBAUGx8XFxcXFx8eFxoaGhoXHh4jJSclIx4vLzMzLy9AQEBAQEBAQEBAQEBAQEABEQ8PERMRFRISFRQRFBEUGhQWFhQaJhoaHBoaJjAjHh4eHiMwKy4nJycuKzU1MDA1NUBAP0BAQEBAQEBAQEBAQP/AABEIAEMALQMBIgACEQEDEQH/xACGAAABBQEAAAAAAAAAAAAAAAAAAgMEBQYBAQADAQEAAAAAAAAAAAAAAAACAwQAARAAAQMDAQYFAwIHAQAAAAAAARECAwAhBBIxQVEyEwVxoSJCFGGRUmIGscFykqIzUxYRAAEDAgQGAwEAAAAAAAAAAAEAEQIhAzFRYRJBgZEiMgRx0UJi/9oADAMBAAIRAxEAPwC6ShKWldSmukskJQlLShKzrMkaFaXEtY1ti97gxoX6uIpnr4GrR8/F1/j1Qn96aadnxoMmPp5EbZYz7XgOHnVZ/wCY7X1NWl/T/wCOr0+HFKEmbhjR0YEGL4srlKEpSUJe23dXHWZVXdpcdro45ppUcrfjY/NK48oc4co43qN+3XyLLGSWxuYJWY5frMPqLSFPHhUjtGGH4uVJMAflSyanNUegO+uz1XprA7PK7PPcIcxxjfqJ0s6esCwts0HzpJkQRMmjVT2iYmA4GhOaukriUxhTySh8c6daMkK0IHN4pxqSlMFwGO4GiUbchLYRV26pbWOdcWHE04IW7ySftXVroNTSuSOgThADUqI3tWCyIQ6HOiGo6HPcR6irrKNq0+yKFpYjABGgYB7QAgTwFLNcLgxrnusGgknwoCTxJRgKp1D5s74yWlpDbeF6e68qc19xQbKr8FS187irp3l5J4LapalF3DafGuhxEh0UwDcFOIUDG/c+Q1yZkQkYdjo/S9o8DY1bY/eu2Tj0zhh/GQFh86x7xCzZIt97T96GyxFqK4uP6UPnVsvXtHDtOn0o43pj+hqt42WJwVsjHDiHA/zqq75nGPDlh0OY17msbLZJPc7QhNgKz2F0hmY5IOlssZO/3DwrWZHbMHIfkSTQh0rSCQCQ2/u0jfap7lkQI7twNcE+1deu3DVRMaAPF3NhhYgLnEBBwANS/mdlT4nyIr35rqLLq41l8yINbEGH0AvF1KAFBemehGidVuny+9MHrgx3GWdGyQG80mbKvygadbkVbcq+eq1SmrovrT9aaf8AHdRRVSmTLen1Wpp5monUXaPyrau/3ZP9A/jvooqb2PzzTrP65LHz70Xmeicu3ctRLp7tv0SiinDw6oD59F//2Q==",
  "coverTitle": "Your store name here",
  "coverSubtitle": "Write a brief explanation about your store. It can be a brief description, slogan, motto, etc.<br>Example: &quot;Your one stop gift shop for any occasion&quot;.",
  "whyusTitle": "Why Choose Us?",
  "whyusContent": "<p>Use this section to provide information for your customers about why your store is the best place to purchase the type of goods you sell. Be sure to highlight the things that make your products and store services unique. For example, are your items made locally, sourced from special ingredients, unique or customized? This is the section to tell your customers how great your products and services are. Free shipping? Let them know here!</p>",
  "quoteContent": "Insert a customer testimonial or a favorite quote that describes your business motto.",
  "quotePersonName": "Mary Smith",
  "quotePersonTitle": "local celebrity",
  "quotePersonImageUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/quote-portrait.png",
  "meetownerTitle": "About",
  "meetownerContent": "<p>Here you can let your customers get to know you. Tell them a little bit about yourself and why you have chosen to create this particular business. Do you have a passion, hobby or life experience that inspired you to create your business? Help customers feel connected to your purpose and inspire trust in your brand.</p>",
  "meetownerOwnerImageUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/owner-pic.png",
  "meetownerOwnerName": "Joan Doe,",
  "meetownerOwnerTitle": "store owner",
  "locationTitle": "Location",
  "locationDescription": "<p>How customers can find your offline store? Give some more details about your location: your address, store hours, and the easiest way to get to you. Leave this section empty if you do not have offline location.</p>",
  "locationAddress": "144 West D Street, Encinitas, CA 92024 USA",
  "locationLong": "-117.2967266",
  "locationLat": "33.0460986",
  "locationHours": "{\"hours\": [{\"dayto\": \"Friday\", \"hourto\": \"9:30 PM\", \"dayfrom\": \"Monday\", \"hourfrom\": \"10:00 AM\"}, {\"hourto\": \"7:00 PM\", \"dayfrom\": \"Saturday\", \"hourfrom\": \"Noon\"}, {\"dayfrom\": \"Sunday\", \"hourfrom\": \"Closed\"}], \"title\": \"Store Hours\"}",
  "contactusFacebook": "https://www.facebook.com/ecwid",
  "contactusTwitter": "https://twitter.com/ecwid",
  "contactusInstagram": "https://www.instagram.com/ecwid",
  "contactusList": "{\"title\": \"Contact us\", \"channels\": [{\"type\": \"phone\", \"title\": \"Phone\", \"value\": \"1-800-555-0123\"}, {\"type\": \"email\", \"title\": \"Email\", \"value\": \"john.doe@example.com\"}], \"channelsTitle\": \"Connect with us\"}",
  "showCoverImage": true,
  "showWhyUs": true,
  "showQuote": true,
  "showInstagram": true,
  "showMeetowner": true,
  "showLocation": true,
  "showContactus": true,
  "cleanUrlsEnabled": true,
  "storeName": "Awesome store 123",
  "allowSearchEnginesIndexing": true,
  "customHeaderHtmlCode": "<meta name='keywords' content='HTML,CSS,XML,JavaScript'>"
}
```

A JSON object of type 'StarterSiteInfo' with the following fields:

#### StarterSiteInfo

Field | Type | Description
----- | ---- | -----------
coverImageUrl | string | URL to the main image, displayed in background when starter site is opened.
coverImageThumbnail | base64 | Thumbnail cover image.
coverImageMobileUrl | string | URL to the main image, displayed in background when starter site is opened on mobiles.
coverImageMobileThumbnail | base64 | Thumbnail cover image for mobile devices
coverTitle | string | The main title displayed on top of cover image
coverSubtitle | string | Subtitle displayed under the `coverTitle`
whyusTitle | string | Title for "Why us?" section
whyusContent | string | Text for "Why us?" section. HTML tags allowed: *p,u,b,i,sup,a*
quoteContent | string | Customer testimonial about a store. HTML tags allowed: *p,u,b,i,sup,a*
quotePersonName | string | Customer name, who left a testimonial
quotePersonTitle | string | Title or occupance of customer with testimonial
quotePersonImageUrl | string | URL to a photo of a customer with testimonial 
meetownerTitle | string | Store owner "About owner" section title
meetownerContent | string | Text for the "About owner" section. HTML tags allowed: *p,u,b,i,sup,a*
meetownerOwnerImageUrl | string | URL to owner photo in the "About owner" section
meetownerOwnerName | string | Store owner's name in the "About owner" section
meetownerOwnerTitle | string | Store owner's occupation/title in the "About owner" section
locationTitle | string | Title of the "Location" block
locationDescription | string | Text for the "Location" block. HTML tags allowed: *p,u,b,i,sup,a*
locationAddress | string | Address line for store's location
locationLong | string | Longitude coordinate of a store
locationLat | string | Lattitude coordinate of a store
locationHours | JSON string of type \<*LocationHoursInfo*\> | Working hours of a store
contactusFacebook | string | URL to store's Facebook page
contactusTwitter | string | URL to store's Twitter profile
contactusPinterest | string | URL to store's Twitter profile
contactusInstagram | string | URL to store's Twitter profile
contactusInstagramId | string | Instagram ID of a store
contactusFoursquare | string | URL to store's Foursquare profile
contactusYelp | string | URL to store's Yelp profile
contactusVk | string | URL to store's Vk profile
contactusTumblr | string | URL to store's Tumblr profile
contactusEtsy | string | URL to store's Etsy profile
contactusGoogle | string | URL to store's Google+ profile
contactusYoutube | string | URL to store's Youtube profile
contactusVimeo | string | URL to store's Vimeo profile
contactusWechat | string | URL to store's Wechat profile
contactusWhatsapp | string | URL to store's Whatsapp profile
contactusTelegram | string | URL to store's Telegram profile
contactusMessenger | string | URL to store's Messenger profile
contactusLine | string | URL to store's Line profile
contactusViber | string | URL to store's Viber profile
contactusList | \<*ContactUsListInfo*\> | Contact a store block
showCoverImage | boolean | `true` if "Cover image" section is shown. `false` otherwise. Default is `true`
showWhyUs | boolean | `true` if "Why Us?" section is shown. `false` otherwise. Default is `true`
showQuote | boolean | `true` if quote section is shown. `false` otherwise. Default is `true`
showInstagram | boolean | `true` if Instagram feed section is shown. `false` otherwise. Default is `true`
showMeetowner | boolean | `true` if "Meet the Owner" section is shown. `false` otherwise. Default is `true`
showLocation | boolean | `true` if store location section is shown. `false` otherwise. Default is `true`
showContactus | boolean | `true` if "Contact Us" section is shown. `false` otherwise. Default is `true`
cleanUrlsEnabled | boolean | `true` if [SEO-friendly URLs](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls) are enabled. `false` otherwise
storeName | string | Store name
allowSearchEnginesIndexing | boolean | `true` if search engines are allowed to index starter site. `false` otherwise
customHeaderHtmlCode | string | Custom HTML added to HEAD tag in starter site. When updating the field, read the current value first and append the new code below. **This field not preprocessed in any way and executed 'as is'. Use caution**

#### LocationHoursInfo

Field | Type | Description
----- | ---- | -----------
title | string | Location working hours title
hours | \<*LocationHoursDetailedInfo*\> | Detailed location working hours information 

#### LocationHoursDetailedInfo

Field | Type | Description
----- | ---- | -----------
dayfrom | string | Day of the week at the start of period
dayto | string | Day of the week at the end of period
hourfrom | string | Opening hours of a working day
hourto | string | Closing hours of a working day

#### ContactUsListInfo

Field | Type | Description
----- | ---- | -----------
title | string | Title of the Contact Us block
channels | Array\<*ContactUsChannelInfo*\> | Details of contact channels
channelsTitle | string | Title of social media accounts block 'connect with us'

#### ContactUsChannelInfo

Field | Type | Description
----- | ---- | -----------
type | string | Type of contact us channel. One of: `"phone"`, `"url"`, `"email"`
title | string | Title of contact us channel
value | string | Value of contact us channel. For example, a phone number or email address

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
403 | Access token doesn't have `update_store_profile` scope
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Create and update starter site content details

Update information displayed on starter site of a store

#### Request

> Request example

```http
PUT /api/v3/4870020/startersite?token=123abcd HTTP/1.1
Host: app.ecwid.com
Cache-Control: no-cache
```

`PUT https://app.ecwid.com/api/v3/{storeId}/startersite?token={token}`

Name | Type    | Description
---- | ------- | --------------
**storeId** |  number | Ecwid store ID
**token** |  string | oAuth token

<aside class="notice">
Parameters in bold are mandatory
</aside>

> Request example (JSON)

```json
{
  "coverImageUrl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/123456/1468931238307.jpg",
  "coverImageMobileUrl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/123456/1468931236454.jpg",
  "coverImageThumbnail": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAQCAwMDAgQDAwMEBAQEBQkGBQUFBQsICAYJDQsNDQ0LDAwOEBQRDg8TDwwMEhgSExUWFxcXDhEZGxkWGhQWFxb/2wBDAQQEBAUFBQoGBgoWDwwPFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhb/wgARCABpAKgDASIAAhEBAxEB/8QAHAAAAQUBAQEAAAAAAAAAAAAABAECAwUGAAcI/8QAGAEAAwEBAAAAAAAAAAAAAAAAAAECAwT/2gAMAwEAAhADEAAAAfCV7unnenSEoqopkcxynpHTSoZB5HLonNBjmyuoWo0p7F5uPu4pjkV09UUhHcaA7Z2KWJO5KDp1RE0loQRHiDhbIlOJX8VFzuHsLDTKZ5g68YnlnpV6YOM1EWW1DaaQ6bwlR6VRMwSaajGPUa+oRRPJn0zqVMFbi5edfQDPJ5clvisegZqY3r0vbLA7d8wm4xuax39Fj8mPZss6LKwoWvBQbJR2GkWoYdVSP6t4v0qvgGzNNTVCiCGNrNjQNqL9Sw6AbOmjMYqlnglAmnPBmoXQkXmLHLHYzl5XezjS3hYkVT2i8joc6tO02WnmvSDaMkzljkRtox0YqQayyOdvhfaMpYpx3TOXirOVCL52dK9AkR0zdOecVNNJaTU85hIQMPu3OXotjnU8jGfDnsIw0QbOdzr/xAArEAABBAIBAgUEAgMAAAAAAAACAAEDBAUREhATBhQVITEgIjJBM0IWMDX/2gAIAQEAAQUCQ9GXF9D14umAlxda9kTLiiDS100tfQzJkSZMmdNJ7jIn/JvZbTu/HknPYktpk/0N0FfvfVnW1v29ltO7aRf6dKnVnsvIIC2kw7XH34JgTAu27o4CEXZOtdH6V8KBjHgQliq+G5pnfwqTB/jtiMrVeyMTx2oZq2KtTt6LbCQvDloxHAWXVnGTQicXBACsC3AmTofZF8iJEnFVAEKmSlxld7Nelj8ZUyL26vqdppTaVxw9cbB0KdeS36RjzOXHTcrGMaEIIrRyZWlkRMikcXo2zrnUtuU8MgHVrGbWKs0YxibohJnrSnLCRzr0zuXrh2GaYJTt8AjCnI1StXvT3TnGGrbxNguxksjJFO+TirxWvEU8sct+5KovPdqxec3tyRGmEmB83N2iyQk1m13FPl7syx94COzcrgoczXimryNeU9qmAQEMap4oDWSrQCR3JBiUbQqC3JWksXrdlpt6PSJQXZ4wmvWTZOo63intS4/OG9KLyw5etRcvtkRE7rGmU6AZpXqwxyC9GEmu1xhF9MwOgdmUxH2y6CH2v1r2qEjSXccxSXQ00d+4pI5oJn+YC1Jmue6w156lnscD+OSB213HYXciGXXSFSa30aGB0Nasq4RRO2gDL854JNMTKvMFqn4cbyVK7FVtl6fQXkaKKrVYWHUmWfu3PS7DDKJDJ7s8hM79GNMaaRd1XLb9v+rKL+WK0Sax7d3a7u13No322UF5K8gOoYyM/TbbDPEcR9GTLTriScCRQGvKnuvWcXjDSHS2LLn7Mac3dVzHuZA68Nh/udb6t8j8/t+jIVEnX9A+P1B+cHxa/j8Sf9ND+Vj8k6//xAAhEQACAQQCAgMAAAAAAAAAAAAAARECECExAyAEEhNBYf/aAAgBAwEBPwEbProjJBnq7ep6siBMm+tlfKlopaqSYrIrhi6MXg0fH78lcfm2cUrF0V7skKlEGB7wQJmBMryJCtIyjWTRkl2mULtN2yjfT//EAB8RAAIDAAICAwAAAAAAAAAAAAERAAIgECEDEzAxQf/aAAgBAgEBPwHBfzAjAZncr4z+y3RWKYU7nuLVQ55MU5fAqXwYanhSgUOB0Zb74QiEUSKy8qXGP//EAD4QAAEDAQQECQoFBQEAAAAAAAEAAgMRBBIhMRMiQWEFEBQgMkJRcYEjMDNScpGhsdHhU2KCksEkNUBz8PH/2gAIAQEABj8C81mt6p/g5ceB89jxbubTz5EMRfTOiF1993WoMB48/BVNOffdamBqJhmZ3lyo2eA9pD8lrW+Kg7FV88WjGNUI4IjHBnWoq/eVojE7Hq0zQMUQPtFCoiz9daTReDSqFmWdHKpG1UNQd6xKw5uqK8TWTWeN7Dma5d60ZLY5HYaja3e9cvsloawtzvOPlN1O1OkhiabnpHuFAPBFzy14rjQHFV6MZJwJGKOmlN8RNc27tH/qeX8ro15AojKBbCd2aa1lotrWbbz0X2Z1o0h6xkWhe+1SP3Uoqy2eu9rVccwCm5NfBZ3u3jJY2WQ+Co+Is70axu3K85lB3rVqsQr7LPIWnqgUV2CyRswpefcwXK7bwjE97PRNLRdZ4IxjhCC6863k2iqkiY04OyBwCuzTin5BVRWyzSttMcDu43Tm0jvUug4NiFXYurlgr1pbPeOUUeqD8USxpssY/Flqq8uhnjPVY8Ci07TWvVGCLYNJE71i6quutFe3BVsz5S0bGnBXLZpg5v4VAm3C8fmkNUXw2y85uzFXHRMd21AWNlb4YLVY1q1TIGNzDXFXSyBm+WRyBDuCqbdHAXn4qnJ7JM2myyBqfNaZ44mV9G0U+CLLNAX1FDI5aHkzaP6181CJfwfNhTK0tCLW2AX6dKS2DD4ow6OM7CVdwxVLTLJuuayPJZTd/O0ICW6RuYAvot/FcY+g7grrpMO4cfRIB7XMC8u1jhvlYFS08ms7O02uvwC0lnt9jrto51U6rqBg1WAYcQs5dR9Lkb9rT1fjh4rRG7pa0ul2s4rXtEUe4tK/ullHtBw/hVZaYZ/9dcFg6vgqVW1dixxX3V7mFo4KkfTO/bStXgSHxtDiv6eyRWf2Xk/NDAvHbhRSQzNAdmOLOlcFFwjGbrpMJbuyQZ/XxTrRLbLkgzBHSKGjldX1CPjVffi6BJ3KmjIWF7xKzB8eI50WHH0T71i13vR0Yz7QCi2OusamqFp68QFVhls4jYz6SRuHtt6J8Rh7lR8UL3Sa3lIgSPeg6WNgLfwxc+S6Lv3ldB37yqN0jU2ISvEZNTeyBRiELWluqcKYrGext77UxFl5r6eqahY/JYYczasysyi2KPp9Ikru4mnesanxVf5X3WR/cuiughPiXN1JNancVsQa2hLsquoq6JnhO36q7IKHvrzfvxZhdILpBVqCtnHgQq8Vx4qx+Dgdo/7+U+F3BsTXN2tc/wCq1WrLzg5jPaR9or9T/kv0Dnf/xAAmEAEAAgIBAwQDAQEBAAAAAAABABEhMUFRYXEQgZGhscHw0eHx/9oACAEBAAE/IX0eWAWORftNMxKSF1ZaQTU2axCzhEY1fUTDkU94d1OxnHdykQiIiJOIbl25Q1zF1ZtU96yvJ8SnMTqEbe2Kl6I3W9zhZcJi0IqZRiKjxL9CMdxpMMVsCazXHw3LLuYYvOiZOYeFNCaVxOBlxTjUItk49DcJTV1iaYmGFOxuvw/EA77Rv9UO2XokrREE7EBdYgauqXsiiGeEogjGPFmcxYBOiIgAnOENgGRMmPNKH03rnEBYsEDeT6gDFM1uUzz7hCZYThT4qBkFdlAjrdrcFktNDtHZedkFWhuCsF6zsQ5xLbEUbxpuo0aEZbQtz2QE2zXjmmv3GlcKbnf3CunSa3Y0K6LZogw0HnjxiiGbWYtBVZ8fUseoWd5WL6BIFVuaU5vJ7wcFutUYqXEwdJzG2k+YRAN6/vBHYLWO9Qr1BxYKT3uUvrF5CbNJ0HMx8K4bnwJBqWde6mZEDAgUpF8hhxc/8BlydH7fau45oRoHzR7TLXdWBvGdV3g60HOr1zgmI5Eyw1xVUfebljKziKqzpCv9mAoPIQMaWjif/vkz89Ft8ztAR/SoNaLoHP1LbsM4KZVvg+zFgmObo+JX1Fop+ZQ1gpZX1Kd8f+kfAXxbKd1UH5SUVPKg/TLjWo8v0hjKSqmC6yyv1Zip4OK8zEwSC/Zde0tCWSnlN0xZ66jNbyj/AJAgnkk/CK16rFt/c1WBXOozD0az8mMPQMp/MoZGv0BBuvwoSAF+UQOLuXdBos/USpVhP+U53N4Bh1pE7uwWX3OwGcrGxVuGr5GVsUdyTaKwzmI6sWL6Qf40aCWFTqr9E4CeJ0PZnmvsEOd23pNmXtDgtxFNesrldnvFKwkJfo7zfNxqMKE5gR+osD1/WM2l8t18mB52YaAeY6wFxprp7X8RQRXRkTjv7bjV/wC6zvvfAi/VCszLdZu+dcy9qlrl/wAaicIOuGddSVVGwA6lXM/V6lEMXItN3XzHfyNBMnhLjvcx6vpr38lKv8jOzADmDMqTwMfZT8zuPnwil1PJafurX4gESJwV1cC7Zqv5YXEN/wB3eOkq/jmU4DYDv7jWlsNqQv8A3tMUnUjd1Wo3IPS5+ZexOvxyQcFg4cJYx4RY+gV7gjhmDs7dZcfQ5bxREcXuijrO11xE5XVYs2H97ytNw9R7x3GOqsarPy4e0NZW5YT9+nuQCoe1g/cQPsstfrF+Jqw7B/Bix3LQK4/M8cPWPmdMe8Tl6O4Noy1EdalEu0ocg97lvY/MtgrzFvIam0x4JaGLS4/4NPvDs40Q9GUfS5iVwvxLd4+mnzN8I/SE4+jf1OiMfWenv4O/0Zn5/QN+n//aAAwDAQACAAMAAAAQ8d5e3VnJeA9A0oSUfcON4x+ApYqZ8KEmwRoHc+6WjAR8lJMNVxEQad8CCX33cF/xJw4jpsQAzhqy8//EAB4RAQEBAQEAAwEBAQAAAAAAAAEAESExEEFRgaHB/9oACAEDAQE/EHyw8tXqS5uNme2o46wvYw5dln20fLj6s55C+iKVykGWNtld6yA2LBjv7/n19xk8YctSXlsvUJkPPfjCY+SAIPQdfznh/YooQ9Nz/mxyTew7rafGQmwvpDPLEoMYmvpCVP7G8hG62E/US9+W/wBkpbYzW/ADxlbJO2F7He/GQRLS2WxcyeqI4sPL/8QAHhEBAQEAAwEBAAMAAAAAAAAAAQARECExQSBRYZH/2gAIAQIBAT8Q5ZD5JF3Z+m0lLSd5+AC/IV2S9VmQUXy3IyfGCE+Wcaiq7q+r0f79idMG+QZPe3j2Ry1+WrH+LuIXO/Yj3A72BSV3IMtBvOFnQmeJ7LR3L+ixbSPRGBxtrgODuNnsAFvUdhx//8QAJhABAAICAQQCAgMBAQAAAAAAAQARITFBUWFxgZGhscEQ0fDh8f/aAAgBAQABPxDIziJ0Qq7LxHTVpxATAZ4NYlEBXay1B2HECKOclzBy8m4DI9kUBRdkB8I5jOm3qEzQyeICLGluOZRQtRLmJ1xBcwb1NZVHVgqWuhhWjfVGabM1RNiqtS90CNAXcCuDOVzMFQ4JuaB10lY5tnEcaqueZfFahA6LVwBwNMMrLhmcq4KsxFZOZvTOZeZnyECBUEuMFdYFeFS1QvapVVKpkWT3ihGPUXaZhsWdEDjtpsj5sp3BziLMdlxcTW130l7oupxAxR4EipxceiqjhQIWCkMysqusRGQjOhBApIgrylRzAhENK0UWq27oooy3gr29w1oK7sJVIZmheYFBRI44jVylAuBbDuY4GVNGIXSFtL4hqqBVVCDRBFdyaI7y2sFCgp9QD6iqNy4R4VTxzERFiytSycxlV/WYzhLPErHbVVWgrED/AG1RDlgvvdwnNa3wKlvEwrWSXPktiVLaJOHMAgWU1/5CVxG2dBcLASwAESRdKmVUBt0la5VxApl7hNx2XviA2AIm9nF1DVku8g5m30UpcBiyF8XxuMNwdG1kGL3dqxUtrWRA0FRYcABypMQDksxA3Stq0K63cRwt2MZlFAFsl57w1bwiQZ8I+0YH4gq6XlnxxiMUkuOABu4vh1xC8JdBCZWKe+ALKuoq8fhT8S0OxZb4yMx8Kpz14XG5bfFI+Q1EG4tWTwxoYYEX8TLi6uI7TI9WQwXS+I+5jueVTM7Rwv3Fnu0TlMHnXBxXEWm6Cye3EQLMOQAzs5m66LbVA7Y1mxcgqAXdQxajk0W1FKyoSIpIFjcwtY9i5Xbc1JsmURoo3TpKmYM5mU23lvllvMCD3n9ka6ItoJlBGvmDaaWB5tL9IY0BkDzwy5GN0SuhDiPCPyIHCSlTdJZDlqxPytrCffQRfaYWA3Zn0l3oFCesKMvlV+CLyq6QCe6JSSV3D5eCCDnKdrm0RjjKaKO6lj3KEbfIps91V8S/VCSdAUAEBtdawwzVYIB6ROWldjcQHQwoEKjQzWSn1GOgwYu9nOItJejHZt0fYQYa6MsdtuJ1V4qveLhYAWD5uoqaKB/kKmS41Zl3EB6Y0L9j7EFjgV3hwvqJFDhsywqzYcSlOMH3FQnsWMnxGGmF8rCGw+IfoOEerPuG6A38mAS8UhZmnpZKt1w0eMh7UQMtUoSALbzZarlZh+2PEA6aBYOqcSrcHbE1+1CoUIFy2IdXBxeVbf37kcKClMfXKYfRosOqY0GrowvUrvLDfdFteIDSj2M+Jcw0YRmAvAhUfqFq0vIxQTjQMl7ETakeaUXFsI79RXfIH1Fi6T90BMcqcnekjCulUzlFUFbYTguRC1jq0V5JgLpwuJccK2yXQ7qvSP8AHEsGC6VvdOyN6f0Z4sAACpD0QuWTKIrICgPUXKkjjOBAqgj5uZnjhv4IyROxe2AQJ3Q/TcFQthQFeqIoLDsATIZ1wBB0bT6ieaWwJVRpy+oroo84gBRtV/4j6qt0h/3GzJxR6C16jIqIBJjVFXQllV4zKP0VvRcN0CXumhcKX8WWj1r1MPn7i9irRQKg9V1ykaBypnjsecmD3DwEEbOcPuXN1zFNAeYK2XVmgoSuCC622YDaIzlAtjgVhACQjxtFoBst9z0T2Xp5igvggnfATuSz8sofaiNSijAogcOeEZZctysBX6JtGvtDqMzJAXYCOVnkQCywYwtaUYLruYYzecGNDs/D7ZnjgIrK0VZSVmzeJaqMrbPzKq1mlpGaUryf9xBk14s/uXoK/wCBmU7n3ccDRdJZ7lXp11IS6Uewmwh0KnnMRrVA/wBgD2kQhulku5Z9y6GF0BXlD7gcJ8TxQuuXpcKN8bwjdQ+UZWKVYGd/6ksELNlr4lqBPmdPaWNeIiHm1nH1CXs1kqVf2UEqQrlufliHTjrZ+Yhbq8NaQKdjiwl3nPYGoL6+BEdAsyRIPm9liNilyGcNWDoRdn4j4/kLomnpNjxNPCfqTd4m3i/lht4P3Ptz8T+J/h9Z/l9f4l/x+Z+80+E/Gfw6E//Z",
  "coverImageMobileThumbnail": "data:image/jpeg;base64,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",
  "coverTitle": "Your store name here",
  "coverSubtitle": "Write a brief explanation about your store. It can be a brief description, slogan, motto, etc.<br>Example: &quot;Your one stop gift shop for any occasion&quot;.",
  "whyusTitle": "Why Choose Us?",
  "whyusContent": "<p>Use this section to provide information for your customers about why your store is the best place to purchase the type of goods you sell. Be sure to highlight the things that make your products and store services unique. For example, are your items made locally, sourced from special ingredients, unique or customized? This is the section to tell your customers how great your products and services are. Free shipping? Let them know here!</p>",
  "quoteContent": "Insert a customer testimonial or a favorite quote that describes your business motto.",
  "quotePersonName": "Mary Smith",
  "quotePersonTitle": "local celebrity",
  "quotePersonImageUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/quote-portrait.png",
  "meetownerTitle": "About",
  "meetownerContent": "<p>Here you can let your customers get to know you. Tell them a little bit about yourself and why you have chosen to create this particular business. Do you have a passion, hobby or life experience that inspired you to create your business? Help customers feel connected to your purpose and inspire trust in your brand.</p>",
  "meetownerOwnerImageUrl": "https://d1howb1wwyap5o.cloudfront.net/startersite/owner-pic.png",
  "meetownerOwnerName": "Joan Doe,",
  "meetownerOwnerTitle": "store owner",
  "locationTitle": "Location",
  "locationDescription": "<p>How customers can find your offline store? Give some more details about your location: your address, store hours, and the easiest way to get to you. Leave this section empty if you do not have offline location.</p>",
  "locationAddress": "144 West D Street, Encinitas, CA 92024 USA",
  "locationLong": "-117.2967266",
  "locationLat": "33.0460986",
  "locationHours": "{\"hours\": [{\"dayto\": \"Friday\", \"hourto\": \"9:30 PM\", \"dayfrom\": \"Monday\", \"hourfrom\": \"10:00 AM\"}, {\"hourto\": \"7:00 PM\", \"dayfrom\": \"Saturday\", \"hourfrom\": \"Noon\"}, {\"dayfrom\": \"Sunday\", \"hourfrom\": \"Closed\"}], \"title\": \"Store Hours\"}",
  "contactusFacebook": "https://www.facebook.com/ecwid",
  "contactusTwitter": "https://twitter.com/ecwid",
  "contactusInstagram": "https://www.instagram.com/ecwid",
  "contactusList": "{\"title\": \"Contact us\", \"channels\": [{\"type\": \"phone\", \"title\": \"Phone\", \"value\": \"1-800-555-0123\"}, {\"type\": \"email\", \"title\": \"Email\", \"value\": \"john.doe@example.com\"}], \"channelsTitle\": \"Connect with us\"}",
  "showCoverImage": true,
  "showWhyUs": true,
  "showQuote": false,
  "showInstagram": true,
  "showMeetowner": true,
  "showLocation": false,
  "showContactus": true,
  "cleanUrlsEnabled": true,
  "storeName": "Awesome store 123",
  "allowSearchEnginesIndexing": true,
  "customHeaderHtmlCode": "<meta name='keywords' content='HTML,CSS,XML,JavaScript'>"
}
```

A JSON object of type 'StarterSiteInfo' with the following fields:

#### StarterSiteInfo

Field | Type | Description
----- | ---- | -----------
coverImageUrl | string | URL to the main image, displayed in background when starter site is opened.
coverImageThumbnail | base64 | Thumbnail cover image.
coverImageMobileUrl | string | URL to the main image, displayed in background when starter site is opened on mobiles.
coverImageMobileThumbnail | base64 | Thumbnail cover image for mobile devices
coverTitle | string | The main title displayed on top of cover image
coverSubtitle | string | Subtitle displayed under the `coverTitle`
whyusTitle | string | Title for "Why us?" section
whyusContent | string | Text for "Why us?" section. HTML tags allowed: *p,u,b,i,sup,a*
quoteContent | string | Customer testimonial about a store. HTML tags allowed: *p,u,b,i,sup,a*
quotePersonName | string | Customer name, who left a testimonial
quotePersonTitle | string | Title or occupance of customer with testimonial
meetownerTitle | string | Store owner "About owner" section title
meetownerContent | string | Text for the "About owner" section. HTML tags allowed: *p,u,b,i,sup,a*
meetownerOwnerName | string | Store owner's name in the "About owner" section
meetownerOwnerTitle | string | Store owner's occupation/title in the "About owner" section
locationTitle | string | Title of the "Location" block
locationDescription | string | Text for the "Location" block. HTML tags allowed: *p,u,b,i,sup,a*
locationAddress | string | Address line for store's location
locationLong | string | Longitude coordinate of a store
locationLat | string | Lattitude coordinate of a store
locationHours | JSON string of type \<*LocationHoursInfo*\> | Working hours of a store
contactusFacebook | string | URL to store's Facebook page
contactusTwitter | string | URL to store's Twitter profile
contactusPinterest | string | URL to store's Twitter profile
contactusInstagram | string | URL to store's Twitter profile
contactusInstagramId | string | Instagram ID of a store
contactusFoursquare | string | URL to store's Foursquare profile
contactusYelp | string | URL to store's Yelp profile
contactusVk | string | URL to store's Vk profile
contactusTumblr | string | URL to store's Tumblr profile
contactusEtsy | string | URL to store's Etsy profile
contactusGoogle | string | URL to store's Google+ profile
contactusYoutube | string | URL to store's Youtube profile
contactusVimeo | string | URL to store's Vimeo profile
contactusWechat | string | URL to store's Wechat profile
contactusWhatsapp | string | URL to store's Whatsapp profile
contactusTelegram | string | URL to store's Telegram profile
contactusMessenger | string | URL to store's Messenger profile
contactusLine | string | URL to store's Line profile
contactusViber | string | URL to store's Viber profile
contactusList | JSON string of type \<*ContactUsListInfo*\> | Contact a store block
showCoverImage | boolean | Set to `false` to hide the "Cover image" section. `true` otherwise. Default is `true`
showWhyUs | boolean | Set to `false` to hide "Why Us?" section. `true` otherwise. Default is `true`
showQuote | boolean | Set to `false` to hide quote section. `true` otherwise. Default is `true`
showInstagram | boolean | Set to `false` to hide Instagram feed section. `true` otherwise. Default is `true`
showMeetowner | boolean | Set to `false` to hide "Meet the Owner" section. `true` otherwise. Default is `true`
showLocation | boolean | Set to `false` to hide store location section. `true` otherwise. Default is `true`
showContactus | boolean | Set to `false` to hide "Contact Us" section. `true` otherwise. Default is `true`
cleanUrlsEnabled | boolean | `true` if [SEO-friendly URLs](https://developers.ecwid.com/api-documentation/seo#seo-friendly-urls) are enabled. `false` otherwise
storeName | string | Store name
allowSearchEnginesIndexing | boolean | `true` if search engines are allowed to index starter site. `false` otherwise
customHeaderHtmlCode | string | Custom HTML added to HEAD tag in starter site. When updating the field, read the current value first and append the new code below. **This field not preprocessed in any way and executed 'as is'. Use caution**

#### LocationHoursInfo

Field | Type | Description
----- | ---- | -----------
title | string | Location working hours title
hours | \<*LocationHoursDetailedInfo*\> | Detailed location working hours information 

#### LocationHoursDetailedInfo

Field | Type | Description
----- | ---- | -----------
dayfrom | string | Day of the week at the start of period
dayto | string | Day of the week at the end of period
hourfrom | string | Opening hours of a working day
hourto | string | Closing hours of a working day

#### ContactUsListInfo

Field | Type | Description
----- | ---- | -----------
title | string | Title of the Contact Us block
channels | Array\<*ContactUsChannelInfo*\> | Details of contact channels
channelsTitle | string | Title of social media accounts block 'connect with us'

#### ContactUsChannelInfo

Field | Type | Description
----- | ---- | -----------
type | string | Type of contact us channel. One of: `"phone"`, `"url"`, `"email"`
title | string | Title of contact us channel
value | string | Value of contact us channel. For example, a phone number or email address

#### Response


> Response example

```json
{
    "id": 1003
}
```


A JSON object of type 'UpdateStatus' with the following fields:

#### UpdateStatus
Field | Type |  Description
------| ---- | --------------
id | number | The store ID of the store where the settings were changed.

#### Errors

> Error response example

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

HTTP Status | Meaning
------------|--------
403 | Access token doesn't have `update_store_profile` scope
415 | Unsupported content-type: expected `application/json` or `text/json`
500 | Server error

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Upload cover image

Upload image to prepare it for a starter site cover. 

<aside class="note">Once you get a successful result, get the JSON response and send it via <a href="https://developers.ecwid.com/api-documentation/starter-site#create-and-update-starter-site-content-details">Update starter site content</a> request to update the starter site cover image.</aside>

> Request example

```http
POST /api/v3/4870020/startersite/cover?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/startersite/cover?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/startersite/cover?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/startersite/cover?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading a starter site cover, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

<aside class="note">
Minimum image dimensions are: 1000х667px and maximun dimenstions are: 1920х1280px.
</aside>

#### Response

> Response example

```json
{
  "coverImageUrl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/123456/1468931238307.jpg",
  "coverImageMobileUrl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/123456/1468931236454.jpg",
  "coverImageThumbnail": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAQCAwMDAgQDAwMEBAQEBQkGBQUFBQsICAYJDQsNDQ0LDAwOEBQRDg8TDwwMEhgSExUWFxcXDhEZGxkWGhQWFxb/2wBDAQQEBAUFBQoGBgoWDwwPFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhYWFhb/wgARCABpAKgDASIAAhEBAxEB/8QAHAAAAQUBAQEAAAAAAAAAAAAABAECAwUGAAcI/8QAGAEAAwEBAAAAAAAAAAAAAAAAAAECAwT/2gAMAwEAAhADEAAAAfCV7unnenSEoqopkcxynpHTSoZB5HLonNBjmyuoWo0p7F5uPu4pjkV09UUhHcaA7Z2KWJO5KDp1RE0loQRHiDhbIlOJX8VFzuHsLDTKZ5g68YnlnpV6YOM1EWW1DaaQ6bwlR6VRMwSaajGPUa+oRRPJn0zqVMFbi5edfQDPJ5clvisegZqY3r0vbLA7d8wm4xuax39Fj8mPZss6LKwoWvBQbJR2GkWoYdVSP6t4v0qvgGzNNTVCiCGNrNjQNqL9Sw6AbOmjMYqlnglAmnPBmoXQkXmLHLHYzl5XezjS3hYkVT2i8joc6tO02WnmvSDaMkzljkRtox0YqQayyOdvhfaMpYpx3TOXirOVCL52dK9AkR0zdOecVNNJaTU85hIQMPu3OXotjnU8jGfDnsIw0QbOdzr/xAArEAABBAIBAgUEAgMAAAAAAAACAAEDBAUREhATBhQVITEgIjJBM0IWMDX/2gAIAQEAAQUCQ9GXF9D14umAlxda9kTLiiDS100tfQzJkSZMmdNJ7jIn/JvZbTu/HknPYktpk/0N0FfvfVnW1v29ltO7aRf6dKnVnsvIIC2kw7XH34JgTAu27o4CEXZOtdH6V8KBjHgQliq+G5pnfwqTB/jtiMrVeyMTx2oZq2KtTt6LbCQvDloxHAWXVnGTQicXBACsC3AmTofZF8iJEnFVAEKmSlxld7Nelj8ZUyL26vqdppTaVxw9cbB0KdeS36RjzOXHTcrGMaEIIrRyZWlkRMikcXo2zrnUtuU8MgHVrGbWKs0YxibohJnrSnLCRzr0zuXrh2GaYJTt8AjCnI1StXvT3TnGGrbxNguxksjJFO+TirxWvEU8sct+5KovPdqxec3tyRGmEmB83N2iyQk1m13FPl7syx94COzcrgoczXimryNeU9qmAQEMap4oDWSrQCR3JBiUbQqC3JWksXrdlpt6PSJQXZ4wmvWTZOo63intS4/OG9KLyw5etRcvtkRE7rGmU6AZpXqwxyC9GEmu1xhF9MwOgdmUxH2y6CH2v1r2qEjSXccxSXQ00d+4pI5oJn+YC1Jmue6w156lnscD+OSB213HYXciGXXSFSa30aGB0Nasq4RRO2gDL854JNMTKvMFqn4cbyVK7FVtl6fQXkaKKrVYWHUmWfu3PS7DDKJDJ7s8hM79GNMaaRd1XLb9v+rKL+WK0Sax7d3a7u13No322UF5K8gOoYyM/TbbDPEcR9GTLTriScCRQGvKnuvWcXjDSHS2LLn7Mac3dVzHuZA68Nh/udb6t8j8/t+jIVEnX9A+P1B+cHxa/j8Sf9ND+Vj8k6//xAAhEQACAQQCAgMAAAAAAAAAAAAAARECECExAyAEEhNBYf/aAAgBAwEBPwEbProjJBnq7ep6siBMm+tlfKlopaqSYrIrhi6MXg0fH78lcfm2cUrF0V7skKlEGB7wQJmBMryJCtIyjWTRkl2mULtN2yjfT//EAB8RAAIDAAICAwAAAAAAAAAAAAERAAIgECEDEzAxQf/aAAgBAgEBPwHBfzAjAZncr4z+y3RWKYU7nuLVQ55MU5fAqXwYanhSgUOB0Zb74QiEUSKy8qXGP//EAD4QAAEDAQQECQoFBQEAAAAAAAEAAgMRBBIhMRMiQWEFEBQgMkJRcYEjMDNScpGhsdHhU2KCksEkNUBz8PH/2gAIAQEABj8C81mt6p/g5ceB89jxbubTz5EMRfTOiF1993WoMB48/BVNOffdamBqJhmZ3lyo2eA9pD8lrW+Kg7FV88WjGNUI4IjHBnWoq/eVojE7Hq0zQMUQPtFCoiz9daTReDSqFmWdHKpG1UNQd6xKw5uqK8TWTWeN7Dma5d60ZLY5HYaja3e9cvsloawtzvOPlN1O1OkhiabnpHuFAPBFzy14rjQHFV6MZJwJGKOmlN8RNc27tH/qeX8ro15AojKBbCd2aa1lotrWbbz0X2Z1o0h6xkWhe+1SP3Uoqy2eu9rVccwCm5NfBZ3u3jJY2WQ+Co+Is70axu3K85lB3rVqsQr7LPIWnqgUV2CyRswpefcwXK7bwjE97PRNLRdZ4IxjhCC6863k2iqkiY04OyBwCuzTin5BVRWyzSttMcDu43Tm0jvUug4NiFXYurlgr1pbPeOUUeqD8USxpssY/Flqq8uhnjPVY8Ci07TWvVGCLYNJE71i6quutFe3BVsz5S0bGnBXLZpg5v4VAm3C8fmkNUXw2y85uzFXHRMd21AWNlb4YLVY1q1TIGNzDXFXSyBm+WRyBDuCqbdHAXn4qnJ7JM2myyBqfNaZ44mV9G0U+CLLNAX1FDI5aHkzaP6181CJfwfNhTK0tCLW2AX6dKS2DD4ow6OM7CVdwxVLTLJuuayPJZTd/O0ICW6RuYAvot/FcY+g7grrpMO4cfRIB7XMC8u1jhvlYFS08ms7O02uvwC0lnt9jrto51U6rqBg1WAYcQs5dR9Lkb9rT1fjh4rRG7pa0ul2s4rXtEUe4tK/ullHtBw/hVZaYZ/9dcFg6vgqVW1dixxX3V7mFo4KkfTO/bStXgSHxtDiv6eyRWf2Xk/NDAvHbhRSQzNAdmOLOlcFFwjGbrpMJbuyQZ/XxTrRLbLkgzBHSKGjldX1CPjVffi6BJ3KmjIWF7xKzB8eI50WHH0T71i13vR0Yz7QCi2OusamqFp68QFVhls4jYz6SRuHtt6J8Rh7lR8UL3Sa3lIgSPeg6WNgLfwxc+S6Lv3ldB37yqN0jU2ISvEZNTeyBRiELWluqcKYrGext77UxFl5r6eqahY/JYYczasysyi2KPp9Ikru4mnesanxVf5X3WR/cuiughPiXN1JNancVsQa2hLsquoq6JnhO36q7IKHvrzfvxZhdILpBVqCtnHgQq8Vx4qx+Dgdo/7+U+F3BsTXN2tc/wCq1WrLzg5jPaR9or9T/kv0Dnf/xAAmEAEAAgIBAwQDAQEBAAAAAAABABEhMUFRYXEQgZGhscHw0eHx/9oACAEBAAE/IX0eWAWORftNMxKSF1ZaQTU2axCzhEY1fUTDkU94d1OxnHdykQiIiJOIbl25Q1zF1ZtU96yvJ8SnMTqEbe2Kl6I3W9zhZcJi0IqZRiKjxL9CMdxpMMVsCazXHw3LLuYYvOiZOYeFNCaVxOBlxTjUItk49DcJTV1iaYmGFOxuvw/EA77Rv9UO2XokrREE7EBdYgauqXsiiGeEogjGPFmcxYBOiIgAnOENgGRMmPNKH03rnEBYsEDeT6gDFM1uUzz7hCZYThT4qBkFdlAjrdrcFktNDtHZedkFWhuCsF6zsQ5xLbEUbxpuo0aEZbQtz2QE2zXjmmv3GlcKbnf3CunSa3Y0K6LZogw0HnjxiiGbWYtBVZ8fUseoWd5WL6BIFVuaU5vJ7wcFutUYqXEwdJzG2k+YRAN6/vBHYLWO9Qr1BxYKT3uUvrF5CbNJ0HMx8K4bnwJBqWde6mZEDAgUpF8hhxc/8BlydH7fau45oRoHzR7TLXdWBvGdV3g60HOr1zgmI5Eyw1xVUfebljKziKqzpCv9mAoPIQMaWjif/vkz89Ft8ztAR/SoNaLoHP1LbsM4KZVvg+zFgmObo+JX1Fop+ZQ1gpZX1Kd8f+kfAXxbKd1UH5SUVPKg/TLjWo8v0hjKSqmC6yyv1Zip4OK8zEwSC/Zde0tCWSnlN0xZ66jNbyj/AJAgnkk/CK16rFt/c1WBXOozD0az8mMPQMp/MoZGv0BBuvwoSAF+UQOLuXdBos/USpVhP+U53N4Bh1pE7uwWX3OwGcrGxVuGr5GVsUdyTaKwzmI6sWL6Qf40aCWFTqr9E4CeJ0PZnmvsEOd23pNmXtDgtxFNesrldnvFKwkJfo7zfNxqMKE5gR+osD1/WM2l8t18mB52YaAeY6wFxprp7X8RQRXRkTjv7bjV/wC6zvvfAi/VCszLdZu+dcy9qlrl/wAaicIOuGddSVVGwA6lXM/V6lEMXItN3XzHfyNBMnhLjvcx6vpr38lKv8jOzADmDMqTwMfZT8zuPnwil1PJafurX4gESJwV1cC7Zqv5YXEN/wB3eOkq/jmU4DYDv7jWlsNqQv8A3tMUnUjd1Wo3IPS5+ZexOvxyQcFg4cJYx4RY+gV7gjhmDs7dZcfQ5bxREcXuijrO11xE5XVYs2H97ytNw9R7x3GOqsarPy4e0NZW5YT9+nuQCoe1g/cQPsstfrF+Jqw7B/Bix3LQK4/M8cPWPmdMe8Tl6O4Noy1EdalEu0ocg97lvY/MtgrzFvIam0x4JaGLS4/4NPvDs40Q9GUfS5iVwvxLd4+mnzN8I/SE4+jf1OiMfWenv4O/0Zn5/QN+n//aAAwDAQACAAMAAAAQ8d5e3VnJeA9A0oSUfcON4x+ApYqZ8KEmwRoHc+6WjAR8lJMNVxEQad8CCX33cF/xJw4jpsQAzhqy8//EAB4RAQEBAQEAAwEBAQAAAAAAAAEAESExEEFRgaHB/9oACAEDAQE/EHyw8tXqS5uNme2o46wvYw5dln20fLj6s55C+iKVykGWNtld6yA2LBjv7/n19xk8YctSXlsvUJkPPfjCY+SAIPQdfznh/YooQ9Nz/mxyTew7rafGQmwvpDPLEoMYmvpCVP7G8hG62E/US9+W/wBkpbYzW/ADxlbJO2F7He/GQRLS2WxcyeqI4sPL/8QAHhEBAQEAAwEBAAMAAAAAAAAAAQARECExQSBRYZH/2gAIAQIBAT8Q5ZD5JF3Z+m0lLSd5+AC/IV2S9VmQUXy3IyfGCE+Wcaiq7q+r0f79idMG+QZPe3j2Ry1+WrH+LuIXO/Yj3A72BSV3IMtBvOFnQmeJ7LR3L+ixbSPRGBxtrgODuNnsAFvUdhx//8QAJhABAAICAQQCAgMBAQAAAAAAAQARITFBUWFxgZGhscEQ0fDh8f/aAAgBAQABPxDIziJ0Qq7LxHTVpxATAZ4NYlEBXay1B2HECKOclzBy8m4DI9kUBRdkB8I5jOm3qEzQyeICLGluOZRQtRLmJ1xBcwb1NZVHVgqWuhhWjfVGabM1RNiqtS90CNAXcCuDOVzMFQ4JuaB10lY5tnEcaqueZfFahA6LVwBwNMMrLhmcq4KsxFZOZvTOZeZnyECBUEuMFdYFeFS1QvapVVKpkWT3ihGPUXaZhsWdEDjtpsj5sp3BziLMdlxcTW130l7oupxAxR4EipxceiqjhQIWCkMysqusRGQjOhBApIgrylRzAhENK0UWq27oooy3gr29w1oK7sJVIZmheYFBRI44jVylAuBbDuY4GVNGIXSFtL4hqqBVVCDRBFdyaI7y2sFCgp9QD6iqNy4R4VTxzERFiytSycxlV/WYzhLPErHbVVWgrED/AG1RDlgvvdwnNa3wKlvEwrWSXPktiVLaJOHMAgWU1/5CVxG2dBcLASwAESRdKmVUBt0la5VxApl7hNx2XviA2AIm9nF1DVku8g5m30UpcBiyF8XxuMNwdG1kGL3dqxUtrWRA0FRYcABypMQDksxA3Stq0K63cRwt2MZlFAFsl57w1bwiQZ8I+0YH4gq6XlnxxiMUkuOABu4vh1xC8JdBCZWKe+ALKuoq8fhT8S0OxZb4yMx8Kpz14XG5bfFI+Q1EG4tWTwxoYYEX8TLi6uI7TI9WQwXS+I+5jueVTM7Rwv3Fnu0TlMHnXBxXEWm6Cye3EQLMOQAzs5m66LbVA7Y1mxcgqAXdQxajk0W1FKyoSIpIFjcwtY9i5Xbc1JsmURoo3TpKmYM5mU23lvllvMCD3n9ka6ItoJlBGvmDaaWB5tL9IY0BkDzwy5GN0SuhDiPCPyIHCSlTdJZDlqxPytrCffQRfaYWA3Zn0l3oFCesKMvlV+CLyq6QCe6JSSV3D5eCCDnKdrm0RjjKaKO6lj3KEbfIps91V8S/VCSdAUAEBtdawwzVYIB6ROWldjcQHQwoEKjQzWSn1GOgwYu9nOItJejHZt0fYQYa6MsdtuJ1V4qveLhYAWD5uoqaKB/kKmS41Zl3EB6Y0L9j7EFjgV3hwvqJFDhsywqzYcSlOMH3FQnsWMnxGGmF8rCGw+IfoOEerPuG6A38mAS8UhZmnpZKt1w0eMh7UQMtUoSALbzZarlZh+2PEA6aBYOqcSrcHbE1+1CoUIFy2IdXBxeVbf37kcKClMfXKYfRosOqY0GrowvUrvLDfdFteIDSj2M+Jcw0YRmAvAhUfqFq0vIxQTjQMl7ETakeaUXFsI79RXfIH1Fi6T90BMcqcnekjCulUzlFUFbYTguRC1jq0V5JgLpwuJccK2yXQ7qvSP8AHEsGC6VvdOyN6f0Z4sAACpD0QuWTKIrICgPUXKkjjOBAqgj5uZnjhv4IyROxe2AQJ3Q/TcFQthQFeqIoLDsATIZ1wBB0bT6ieaWwJVRpy+oroo84gBRtV/4j6qt0h/3GzJxR6C16jIqIBJjVFXQllV4zKP0VvRcN0CXumhcKX8WWj1r1MPn7i9irRQKg9V1ykaBypnjsecmD3DwEEbOcPuXN1zFNAeYK2XVmgoSuCC622YDaIzlAtjgVhACQjxtFoBst9z0T2Xp5igvggnfATuSz8sofaiNSijAogcOeEZZctysBX6JtGvtDqMzJAXYCOVnkQCywYwtaUYLruYYzecGNDs/D7ZnjgIrK0VZSVmzeJaqMrbPzKq1mlpGaUryf9xBk14s/uXoK/wCBmU7n3ccDRdJZ7lXp11IS6Uewmwh0KnnMRrVA/wBgD2kQhulku5Z9y6GF0BXlD7gcJ8TxQuuXpcKN8bwjdQ+UZWKVYGd/6ksELNlr4lqBPmdPaWNeIiHm1nH1CXs1kqVf2UEqQrlufliHTjrZ+Yhbq8NaQKdjiwl3nPYGoL6+BEdAsyRIPm9liNilyGcNWDoRdn4j4/kLomnpNjxNPCfqTd4m3i/lht4P3Ptz8T+J/h9Z/l9f4l/x+Z+80+E/Gfw6E//Z",
  "coverImageMobileThumbnail": "data:image/jpeg;base64,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"
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus

Field | Type |  Description
----- | -----| ------------
coverImageUrl | string | URL to the main image, displayed in background when starter site is opened.
coverImageThumbnail | base64 | Thumbnail cover image.
coverImageMobileUrl | string | URL to the main image, displayed in background when starter site is opened on mobiles.
coverImageMobileThumbnail | base64 | Thumbnail cover image for mobile devices

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/json` or `text/json`
422 | The uploaded file is not an image
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

### Upload owner portrait image

Upload image to prepare it for an owner portrait. 

<aside class="note">Once you get a successful result, get the JSON response and send it via <a href="https://developers.ecwid.com/api-documentation/starter-site#create-and-update-starter-site-content-details">Update starter site content</a> request to update the starter site owner portrait.
</aside>

> Request example

```http
POST /api/v3/4870020/startersite/ownerportrait?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/startersite/ownerportrait?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/startersite/ownerportrait?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/startersite/ownerportrait?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading a starter site owner portrait, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

<aside class="note">
Minimum image dimensions are: 288х288px.
</aside>

#### Response

> Response example

```json
{
  "meetownerownerimageurl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/1003/1468931549471.jpg"
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus

Name | Type    | Description
---- | ------- | -----------
meetownerownerimageurl | string | URL to owner photo in the "About owner" section

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/octet-stream`
422 | The uploaded file is not an image
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message


### Upload quote person image

Upload starter site quote person image to prepare it.

<aside class="note">Once you get a successful result, get the JSON response and send it via <a href="https://developers.ecwid.com/api-documentation/starter-site#create-and-update-starter-site-content-details">Update starter site content</a> request to update the starter site quote person image.
</aside>

> Request example

```http
POST /api/v3/4870020/startersite/quoteperson?token=123456789abcd HTTP/1.1
Host: app.ecwid.com
Content-Type: image/jpeg
Cache-Control: no-cache

binary data
```

> PHP Example

```php
$file = file_get_contents('image.jpg');
$url = 'https://app.ecwid.com/api/v3/1003/startersite/quoteperson?token=abcdefg123456';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_POST,1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $file);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: image/jpeg;'));

$result = curl_exec($ch);
curl_close ($ch);
```

> Python Example

```python
import requests

request_url = "https://app.ecwid.com/api/v3/1003/startersite/quoteperson?token=abcdefg123456"

image_file_data = open('image.jpg', 'rb').read()

result = requests.post(request_url,data=image_file_data)

print(result.status_code)
```

`POST https://app.ecwid.com/api/v3/{storeId}/startersite/quoteperson?token={token}&externalUrl={externalUrl}`

Name | Type    | Description
---- | ------- | -----------
**storeId** |  number | Ecwid store ID
**token** |  string |  oAuth token
externalUrl | string | External file URL available for public download. If specified, Ecwid will ignore any binary file data sent in a request

When uploading a starter site owner portrait, the image itself needs to be sent in the body of your request in a form of binary data. The file that you wish to upload needs to be prepared for that format and then sent to Ecwid API endpoint. 

Alternatively, you can specify an `externalURL` to your file as a request parameter and Ecwid will download it from there.

<aside class="note">
Minimum image dimensions are: 128х128px.
</aside>

#### Response

> Response example

```json
{
  "quotepersonimageurl": "https://s3.amazonaws.com/images.ecwid.com/startersite/images/1003/1468931800030.jpg"
}
```

A JSON object of type 'UploadStatus' with the following fields:

#### UploadStatus
Name | Type    | Description
---- | ------- | -----------
quotepersonimageurl | string | URL to a photo of a customer with testimonial 

#### Errors

> Error response example 

```http
HTTP/1.1 500 Server Error
Content-Type application/json; charset=utf-8
```

In case of error, Ecwid responds with an error HTTP status code and, optionally, JSON-formatted body containing error description

#### HTTP codes

**HTTP Status** | Description
--------- | -----------| -----------
400 | Request parameters are malformed
415 | Unsupported content-type: expected `application/octet-stream`
422 | The uploaded file is not an image
500 | Uploading of the image file failed or there was an internal server error while processing a file

#### Error response body (optional)

Field | Type |  Description
--------- | ---------| -----------
errorMessage | string | Error message

