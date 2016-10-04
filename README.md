# MSDCRM JS MOBILE API

This is a personal and unofficial documentation for MSDCRM Javascript Mobile API that aims to fulfill the lack of API's specification. The direct reference to this API *is not supported* by Microsoft(r) but in real life scenarios the default behavior provided by the 2013 version may not be useful in many cases. This demand us to customize the application to provide a richer and useful experience to the end user.

Since the use of those methods and tools are not supported, use it at your own risk. No warranties provided.

---
### Table of contents

1. [Mscrm.Utilities](#mscrmutilities)
    1. [clearAllHandlersInSubtree(element)](#clearallhandlersinsubtreeelement)
1. [Mscrm.FormControlInputBehavior](#mscrmformcontrolinputbehavior)
    1. [GetBehavior(id)](#getbehaviorid)
1. [Mscrm.InlinePresenceLookupUIBehavior](#mscrminlinepresencelookupuibehavior)
    1. [prototype.set_disableInlineLookup(value)](#prototype.setdisableinlinelookupvalue)
1. [Mscrm.ReadFormUtilities](#mscrm.readformutilities)
    1. [openLookup(resolved,domEvent)](#openlookupresolveddomevent)

---
## Mscrm.Utilities
#### clearAllHandlersInSubtree(element)
- element: the DOM element to be cleared

Removes all the attached event handlers from the `_events` property and from the element itself. The `element` argument must be the "_i" element variation in order to this method work. Example: `regardingobjectid_i`.

Usage example:
```js
var $element = document.querySelector("#regardingobjectid_i");
Mscrm.Utilities.clearAllHandlersInSubtree($element);
```

## Mscrm.FormControlInputBehavior
### GetBehavior(id)
- id: the DOM element's id to get the behavior

Returns the behavior related to the control passed as argument. It allows us to access the inner methods to manage de control's behavior, like lookup's behaviors and so on.

Could not map all the possible return types since it is Object, but using lookups as an example, the returned type is [`Mscrm.InlinePresenceLookupUIBehavior`](#mscrm.inlinepresencelookupuibehavior).

## Mscrm.InlinePresenceLookupUIBehavior
### prototype.set_disableInlineLookup(value)
- value: a boolean indicating if inline lookup must be enabled (false) or disabled (true)

Changes the lookup component behavior as inline or not. If it is inline, the pre-search window is always shown and the user must click *Lookup more records* to search. If it is not inline, when the control is clicked the search window is immediately shown, allowing the user to begin to search straight away.

## Mscrm.ReadFormUtilities
### openLookup(resolved,domEvent)
- resolved: a boolean indicating if the action should be performed (yet under discover...)
- domEvent: the `Sys.UI.DomEvent(event)` related to the action

This method usage was first noticed in the *Lookup more records* link from the lookup pre-search window. This link opens the lookup search form when clicked. This is the default action attached to this element's click event.
