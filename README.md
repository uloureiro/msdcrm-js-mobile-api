# MSDCRM JS MOBILE API

This is a personal and unofficial documentation for MSDCRM Javascript Mobile API that aims to fulfill the lack of API's specification. The direct reference to this API *is not supported* by Microsoft(r) but in real life scenarios the default behavior provided by the 2013 version may not be useful in many cases. This demand us to customize the application to provide a richer and useful experience to the end user.

Since the use of those methods and tools are not supported, use it at your own risk. No warranties provided.

---
### Table of contents

 1. [Mscrm.Utilities](#Mscrm.Utilities)
    1. [clearAllHandlersInSubtree(element)](#clearAllHandlersInSubtree(element))
1. [Mscrm.FormControlInputBehavior](#Mscrm.FormControlInputBehavior)
    1. [GetBehavior(id)](#GetBehavior(id))
1. [Mscrm.InlinePresenceLookupUIBehavior](#Mscrm.InlinePresenceLookupUIBehavior)
    1. prototype
1. [Mscrm.ReadFormUtilities](#Mscrm.ReadFormUtilities)
    1. [openLookup(resolved,domEvent)](#openLookup(resolved,domEvent))
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

Could not map all the possible return types since it is Object, but using lookups as an example, the returned type is `Mscrm.InlinePresenceLookupUIBehavior`.

## Mscrm.ReadFormUtilities
### openLookup(resolved,domEvent)
- resolved: a boolean indicating if the action should be performed (yet under discover...)
- domEvent: the `Sys.UI.DomEvent(event)` related to the action

This method usage was first noticed in the *Lookup more records* link from the lookup pre-search window. This link opens the lookup search form when clicked. This is the default action attached to this element's click event.
