# MSDCRM JS MOBILE API

This is a personal and unofficial documentation for MSDCRM Javascript Mobile API that aims to fulfill the lack of API's specification. The direct reference to this API *is not supported* by Microsoft(r) but in real life scenarios the default behavior provided by the 2013 version may not be useful in many cases. This demand us to customize the application to provide a richer and useful experience to the end user.

Since the use of those methods and tools are not supported, use it at your own risk. No warranties provided.

---

### Table of contents

1. [Mscrm.FormControlInputBehavior](#mscrmformcontrolinputbehavior)
    1. [GetBehavior(id)](#getbehaviorid)
1. [Mscrm.InlinePresenceLookupUIBehavior](#mscrminlinepresencelookupuibehavior)
    1. [prototype.onLookup(domEvent)](#prototypeonlookupdomevent)
    1. [prototype.set_disableInlineLookup(value)](#prototypesetdisableinlinelookupvalue)
    1. [prototype.set_lookupTypes(value)](#prototypesetlookuptypesvalue)
    1. [showInlineLookupMenu()](#showinlinelookupmenu)
1. [Mscrm.MobileProgressBar](#mscrmmobileprogressbar)
    1. [hideIndefiniteProgressBar()](#hideindefiniteprogressbar)
    1. [showIndefiniteProgressBar()](#showindefiniteprogressbar)
1. [Mscrm.ReadFormUtilities](#mscrm.readformutilities)
    1. [openLookup(resolved,domEvent)](#openlookupresolveddomevent)
1. [Mscrm.Utilities](#mscrmutilities)
    1. [clearAllHandlersInSubtree(element)](#clearallhandlersinsubtreeelement)
1. [Sys.UI.DomEvent](#sysuidomevent)
    1. [addHandler(a,d,e,g)](#addhandleradeg)
1. [lookupinfo.aspx](#lookupinfoaspx)
    1. [Adding a custom view to mobile Lookup controls (bug)](#addingacustomviewtmobilelookupcontrolsbug)

---

## Mscrm.FormControlInputBehavior
### GetBehavior(id)
- id: the DOM element's id to get the behavior

Returns the behavior related to the control which id was passed as argument. It allows us to access the inner methods to manage de control's behavior, like lookup's behaviors and so on.

Could not map all the possible return types since it is Object, but using lookups as an example, the returned type is [`Mscrm.InlinePresenceLookupUIBehavior`](#mscrm.inlinepresencelookupuibehavior).

Usage example:
```js
var behavior = Mscrm.FormControlInputBehavior.GetBehavior("regardingobjectid_i");
```

## Mscrm.InlinePresenceLookupUIBehavior
### prototype.OnLookup(domEvent)
- domEvent: the [`Sys.UI.DomEvent(event)`](#sysuidomeventevent) related to the action

This is the default action attached to the click event of the lookup control when it is an inline lookup in mobile form. The action triggers the lookup pre-search and shows the pre-search window. It would be able to perform a textual search against an inputed value but the field is locked for typing (it actually performs a search against an empty string...).

### prototype.get_lookupTypes()

Returns a string with a comma separated values list with the types of entities supported by the lookup control.

### prototype.set_disableInlineLookup(value)
- value: a boolean indicating if inline lookup must be enabled (false) or disabled (true)

Changes the lookup component behavior as inline or not. If it is inline, the pre-search window is always shown and the user must click *Lookup more records* to search. If it is not inline, when the control is clicked the search window is immediately shown, allowing the user to begin to search straight away.

### prototype.set_lookupTypes(value)
- value: comma separated values list of entity type codes

Sets the types of entities supported by the lookup control.

Usage example:
```js
//This will set the lookup to accept Account and Contact
Mscrm.FormControlInputBehavior.GetBehavior("regardingobjectid_i").set_lookupTypes("1,2");
```

### showInlineLookupMenu()
Triggers the lookup's pre-search window, performing a query against the value in the input (if applicable).

## Mscrm.MobileProgressBar
### hideIndefiniteProgressBar()
Hides the *Loading* overlay used to display an ongoing process to the end user.
Usage example:
```js
// This will hide the 'Loading' overlay
Mscrm.MobileProgressBar.hideIndefiniteProgressBar();
```
### showIndefiniteProgressBar()
Shows the *Loading* overlay used to display an ongoing process to the end user.
Usage example:
```js
// This will show the 'Loading' overlay
Mscrm.MobileProgressBar.showIndefiniteProgressBar();
```

## Mscrm.ReadFormUtilities
### openLookup(resolved,domEvent)
- resolved: a boolean indicating if the action should be performed (yet under discover...)
- domEvent: the [`Sys.UI.DomEvent(event)`](#sysuidomeventevent) related to the action

This method usage was first noticed in the *Lookup more records* link from the lookup pre-search window. This link opens the lookup search form when clicked. This is the default action attached to this element's click event.

## Mscrm.Utilities
#### clearAllHandlersInSubtree(element)
- element: the DOM element to be cleared

Removes all the attached event handlers from the `_events` property and from the element itself. The `element` argument must be the "_i" element variation in order to this method work. Example: `regardingobjectid_i`.

Usage example:
```js
var $element = document.querySelector("#regardingobjectid_i");
Mscrm.Utilities.clearAllHandlersInSubtree($element);
```

## Sys.UI.DomEvent
#### addHandler(a,d,e,g)
- a: the element to attach handler
- d: the event name to be attached (like `"click"`,`"blur"`, etc)
- e: handler function delegate 
- g: boolean indicating if the handlers must be disposed (?uncertain)

Usage example:
```js
var $regardingobjectid_i = document.querySelector("#regardingobjectid_i");
Sys.UI.DomEvent.addHandler(
    $regardingobjectid_i,
    "click",
    function() {
        console.log("clicked!");
    }, 
    false);
```
## lookupinfo.aspx
#### Adding a custom view to mobile Lookup controls (bug)
Two adjustments are needed in lokupinfo.aspx JS (C:\Program Files\Microsoft Dynamics CRM\CRMWeb\_controls\lookup\lookupinfo.aspx).

Before:
```js
// Within function OnRecordTypeChange(oSelect, bFromOnLoad), find  the following piece of code.
for (var i = 0; i < customViews.length; i++) {
    if (typeCode == customViews[i].recordType) {
            var firstChild = XUI.Html.DomUtils.GetLastChild(tdViewSelector);
        }
        else {
            var firstChild = XUI.Html.DomUtils.GetFirstChild(tdViewSelector);
        }
    var tempItem = firstChild.options[firstChild.options.length - 1 - i];
    tempItem.text = customViews[i].name;
    tempItem.value = customViews[i].id;
    tempItem.setAttribute('Type', Mscrm.EntityTypeCode.SavedQuery);
    }
}

if (oSelect.tagName == "SELECT") {
    XUI.Html.DomUtils.GetFirstChild(tdViewSelector).value = oSelect.options[oSelect.selectedIndex].getAttribute("guid");
} 
else {
    if(Mscrm.Utilities.isMobileRefresh()) {
        XUI.Html.DomUtils.GetLastChild(tdViewSelector).value = oSelect.getAttribute("guid");
    }
    else {
        XUI.Html.DomUtils.GetFirstChild(tdViewSelector).value = oSelect.getAttribute("guid");
    }
}
```

After:
```js
//Then change it to match the following.
for (var i = 0; i < customViews.length; i++) {
    if (typeCode == customViews[i].recordType) {
        //the following conditional is added to behave in a specific way when dealing with mobile
        if(Mscrm.Utilities.isMobileRefresh()) {
            var firstChild = XUI.Html.DomUtils.GetLastChild(tdViewSelector);
        }
        else {
            var firstChild = XUI.Html.DomUtils.GetFirstChild(tdViewSelector);
        }
        var tempItem = firstChild.options[firstChild.options.length - 1 - i];
        tempItem.text = customViews[i].name;
        tempItem.value = customViews[i].id;
        tempItem.setAttribute('Type', Mscrm.EntityTypeCode.SavedQuery);
    }
}

if (oSelect.tagName == "SELECT") {
    XUI.Html.DomUtils.GetFirstChild(tdViewSelector).value = oSelect.options[oSelect.selectedIndex].getAttribute("guid");
}
else {
    //the following conditional is added to behave in a specific way when dealing with mobile
    if(Mscrm.Utilities.isMobileRefresh()) {
        XUI.Html.DomUtils.GetLastChild(tdViewSelector).value = oSelect.getAttribute("guid");
    }
    else {
        XUI.Html.DomUtils.GetFirstChild(tdViewSelector).value = oSelect.getAttribute("guid");
    }
}
```
