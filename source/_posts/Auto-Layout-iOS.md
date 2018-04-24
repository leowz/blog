---
title: Auto Layout iOS
date: 2017-05-02 15:58:15
tags: autolayout, iOS
---
`May 2 2017`

![](source/upload/image/constraintEquation.png)

Generally, there are two ways of layout views in iOS apps. The old way, frame based layout and the new way, auto Layout. 
Before iOS 6 or so, we only have frame based layout system for iOS. In Frame based layout we have *programmatically set frame* and *autoresizing mask*.
At iOS 6 and more recently at iOS 9, we finally have auto layout, which is similar to relative layout, and anchors which is an easy way to define constraints in auto layout. In Auto Layout  we can define all the relative relations between views by a couple of constraint equations. we have three ways to implement these equations for any view, we can define the layout constrains by *ASCII String* or add the relationship by *NSLayoutConstraint* class or we can just set anchors from *NSLayoutAnchor* class for each view.

In this article, we focus on auto layout and especially on setting anchors to view to define constraints equation because its the most easy and recommended way at the moment.  

# Some words on frame based layout
Traditionally,especially before iOS 6, apps layout their subview by setting frame property directly. Because at early age, there are small arrange of screen size, so there is no much work on the layout view side. But this is a fairly original way to layout views.

## how it works
User can set frame property from the init time of view and also at runtime at any moment he likes.
In general, view are positioned by their frame property at the init time. However, we can also change the frame by override `layoutSubviews()` and by call `setNeedsLayout()` or `layoutIfNeeded()` to update layout changes. 

However, in most cases,  a view’s frame should be the result of the Auto Layout process, not an input variable.

# Auto Layout
After iOS 6, we have the auto layout system which is a similar version to Android relative layout. Here we can use a constraints equation to define a relation from the view it function on to another view it refers. So it needs at least 4 equations to define precisely the position and the scale of a view. 
| `objectView.attribute1 =(<,>) multiplier * referedView.attribute2 + constant`

## ASCII String auto layout
Use ASCII-art like string to define constraints. By using method:
`NSLayoutConstraint.constraints(withVisualFormat: formatString, options: .alignAllTop, metrics: nil, views: views)`

## NSLayoutConstraint Class
Set a constraint by assigning all the params necessary.
`NSLayoutConstraint(item: myView, attribute: .height, relatedBy: .equal, toItem: myView, attribute:.width, multiplier: 2.0, constant:0.0)`

## Layout Anchors
Use NSLayoutAnchor class. It is a factory class for creating NSLayoutConstraint objects. We can set anchors directly on each view.
`subview.leadingAnchor.constraint(equalTo: margins.leadingAnchor)`

## UILayoutPriority
Very instance of NSLayoutConstraint has a property of `UILayoutPriority` which is in fact a float value. `typealias UILayoutPriority = Float` from 0 to 1000. The bigger the higher in terms of priorty.

This property is used to determine the priority between different constraints. 
When programmer set constraints for an object, if two constraints conflict with each other, the one with bigger priority preveils.
Programmer can also set multiple constraints that conflict with each other but with different priorities. And there will be no error, because of priority. In this way, if some views will be deleted in the future and the constraints with them will also be removed, and the rest of views will be working well because of contraints of lowere prioriy will be in effect.


# Auto Resize
Auto resize is another auto resize layout technology that is exclusive to Auto Layout. They are enemies, not friends. However, auto resize is kinds of an easy and flexible solution to resolve layout problem.

The idea is to use an struct `UIViewAutoresizing` and the UIView hierarchy to solve the problem. Each view is flexible in terms of size and position to its parent view.

Just set each views `autoresizingMask` property to these values from `UIViewAutoresizing` struct.

## UIViewAutoResizing

```
public struct UIViewAutoresizing : OptionSet {

    public init(rawValue: UInt)

    public static var flexibleLeftMargin: UIViewAutoresizing { get }

    public static var flexibleWidth: UIViewAutoresizing { get }

    public static var flexibleRightMargin: UIViewAutoresizing { get }

    public static var flexibleTopMargin: UIViewAutoresizing { get }

    public static var flexibleHeight: UIViewAutoresizing { get }

    public static var flexibleBottomMargin: UIViewAutoresizing { get }
}
```



