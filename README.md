# showcar-tracking

Tracking library for ShowCar-based pages


## Track actions that cause page loads
If you need to track a click on an element that will cause the browser to load another page, you should use
the query parameters ipl and ipc. As you can see here:

    http://www.autoscout24.de/auto-verkaufen/?ipc=cc:insertion-home&ipl=teaser

* ipc: Group or positioning of a feature under test
* ipl: Describes the feature itself

Example: Tracking of the CMS box containing price estimation IPC: home-cmsbox IPL: vehicle-evaluation


GTM will automatically take care of tracking the given values. You just have to insert the two parameters into the URL of the
target of the action. e.g. in the href-attribute of a link.

## Tracking with custom elements

It's possible to do all the tracking with custom HTML elements. In order to accomplish that, you'll have to use the <as24-tracking> element.
To control tracking you use a set of attributes.

There are a couple of common attributes that are used to control the behavior of the custom element:

* type: type of tracking. Possible values are: pagename, gtm or dealer
* action: specific type of tracking. Possible values are: click, set, pageview, listview
* as24-tracking-value: JSON object with key-value pairs to use as bulk set (can be used in combination with attributes)
* as24-tracking-click-target: target selector for click tracking 

All other attributes are used as key-value pairs for tracking. They override values given by the as24-tracking-value attribute.

If you need to use upper case characters (according to the gtm variable) inside an attribute name, you need to escape them with a leading dash.
Example: `group_myTestAttribute` is written as `<as24-tracking group_my-test-attribute="value">`

### Page view

Custom element way
    
    <as24-tracking type="pagename" country="de" market="all-webapp" category="all" />
    <as24-tracking type="gtm" action="pageview" />

JavaScript way

    ut.push(['pagename', {"country":"de","market":"all-webapp","category":"all","pageid":"home","layer":"","attribute":null,"group":null,"environment":"live","language":"de"}]);
    ut.push(['gtm', 'pageview']);    

### Click tracking (without page load)

Custom element way

    <as24-tracking type="gtm" action="click" as24-tracking-click-target="#myButton" linkgroup="HackList" linkid="V1 (abundance)" />
    
JavaScript way

    ut.push(['gtm', 'click',{linkgroup:'HackList',linkid:'V1 (abundance): ' + itemId + '|' + abundance}]);

### Extended click tracking (e.g. for page load)

Click tracking is possible for any `as24-tracking` tag, simply add the attribute `as24-tracking-click-target`.
Its value needs to be a valid selector.

### Setting values

Custom element way

    <as24-tracking type="gtm" action="set" test_experiments.tt5="NetGross:Variation1" />

JavaScript way

    ut.push(['gtm', 'set', {'test_experiments.tt5': 'NetGross:Variation1'}]);

### Dealer tracking

Custom element way

    <as24-tracking type="dealer" action="listview" as24-tracking-value="[281705316,281702707,281462097,281725748,237276348,281667368,281673373,281661776,281555953,281095563]"/>
    
JavaScript way

    ut.push(['dealer', 'listview', [281705316,281702707,281462097,281725748,237276348,281667368,281673373,281661776,281555953,281095563]]);
