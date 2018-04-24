---
title: Core Data And Core Animation
date: 2016-04-30 00:45:25
tags:
---
### Five step to save and fetch DATA in coreData

#### Save data to coreData:
1. Get the managedObjectContext from the AppDelegate

2. Get the entity from the managedObjectContext obtained

3. Link a connection between a variable and the managedObject in the MOcontext. Here means define a variable which is constructed using NSMangedObjects construct function with entity equal to the entity obtained from managedObjectContext.

4. Set the value for any attributes in the managedObject,Save any data to the Object.

5. Save the managedContext,using managedContext.save() throw function.

#### Fetch data from coreData:
1. Get the managedObjectContext from the AppDelegate
2. Create a fetching request
           let fetchRequest = NSFetchRequest(entityName: "Person");
3. ExecuteFetchRequest
           try fetchedResults = managedContext.executeFetchRequest(fetchRequest)
  fetchedResults is the managedObject to handle

#### CoreData Stack:
1. ##### NSManagedObjectContext:    
  An instance of NSManagedObjectContext represents a single “object space” or scratch pad in an application. Its primary responsibility is to manage a collection of managed objects.The managed object context (NSManagedObjectContext) is the object that your application will be interacting with the most, and therefore it is the one that is exposed to the rest of your application.

2. ##### NSPersistentStoreCoordinator:      
    Instances of NSPersistentStoreCoordinator associate persistent stores (by type) with a model (or more accurately, a configuration of a model) and serve to mediate between the persistent store or stores and the managed object context or contexts.

3. ##### NSManagedObjectModel:An NSManagedObjectModel object describes a schema—a collection of entities (data models) that you use in your application.The model contains one or more NSEntityDescription objects representing the entities in the schema.

4. ##### NSPersistentStore:
  representing the external storage
<!--more-->
#### Saving NSManagedObject Instances

OBJECTIVE-C
```objective-c
NSError *error = nil;
 if ([[self managedObjectContext] save:&error] == NO) {
NSAssert(NO, @"Error saving context: %@\n%@", [error localizedDescription], [error userInfo]);
}
```
SWIFT
```swift
do {
try managedObjectContext.save()
} catch {
fatalError("Failure to save context: \(error)")
}
```
#### Fetching NSManagedObject Instances
OC
```objective-c
NSManagedObjectContext *moc = …;
NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@"Employee"];
NSError *error = nil;
NSArray *results = [moc executeFetchRequest:request error:&error];
if (!results) {
DLog(@"Error fetching Employee objects: %@\n%@", [error localizedDescription], [error userInfo]);
abort();
}
```
Swift
```swift      
let moc = managedObjectContext
let employeesFetch = NSFetchRequest(entityName: "Employee")
do {
let fetchedEmployees = try moc.executeFetchRequest(employeesFetch) as! [AAAEmployeeMO]
} catch {
fatalError("Failed to fetch employees: \(error)")
}
```
#### Filtering Results
OC
```objective-c
NSString *firstName = @"Trevor";
[fetchRequest setPredicate:[NSPredicate predicateWithFormat:@"firstName == %@", firstName]];
```
Swift
```swift
let firstName = "Trevor"
fetchRequest.predicate = NSPredicate(format: "firstName == %@", firstName)
```

### Animation
Animation available in iOS
- Animating UIView Properties:frame,transform,alpha
- Core Animation framework:layer, the window from views to coreAnimation
- OpenGL:3D
- SpriteKit: 2.5D
- Dynamic Animation:Physics based

##### UIView
animations from UIView function
- class func animationWithDuration(duration:NSTimeInterval,delay:NSTimeInterval,options:UIViewAnimationOptions,animation:()->Void,completion:((finished:Bool)->Void)?)
OC
```objective-c
UIView.animationWithDuration(3.0,delay:2.0 options:[UIViewAnimationOptions.CurveLinear],animations:{block},completion:{block})
```
