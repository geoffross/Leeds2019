# Lab Guide

## Table of Contents
1. [ Applying a Basic Theme to The Cireson Service Manager Portal ](#themer)
2. [ More Specific Styling ](#css)
3. [ Basic Javascript Customisation ](#js)
4. [ Creating a Custom Task ](#task)
5. [ Advanced Task Intergrating with Remote Support ](#advancedtask)
<a name="themer"></a>
## 1. Applying a Basic Theme to The Cireson Service Manager Portal

We're going to apply a Portal theme in order to set our basic colour scheme in the Portal.

1. Load Portal at http://cset-a-2016smms/ using Chrome. Note the Out of Box colors
1. Head to http://portalthemer.cireson.com/
1. Select the latest version from the selector at the top of the page.
1. Select same basic colours.
1. Click 'Download Theme CSS' to drop out your CSS file.
1. Copy it into your CustomSpace folder at C:\inetpub\CiresonPortal\CustomSapce
1. Edit custom.css from CustomSpace.
1. Add the following at the top.
```css
@import 'custom.theme.min.css';
```
1. Refresh your Portal to see your new theme.

<a name="css"></a>
## 2. More Specific Styling

We're going to change the appearance of a particualr element, that cannot specifically be done using the themer tool. In this lab were going to change the color of the header bar, discover its unique selector and add a CSS rule.

1. Right click on the element you want to customise and choose Inspect.

![Inspect Element][inspect]


<a name="js"></a>
## 3. Basic Javascript Customisation

<a name="task"></a>
## 4. Creating a Custom Task

<a name="advancedtask"></a>
## 5. Advanced Task Intergrating with Remote Support



[inspect]: https://github.com/geoffross/Leeds2019/raw/master/src/common/Images/Inspect.png "Inspect Element"