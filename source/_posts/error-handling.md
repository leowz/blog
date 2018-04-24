---
title: Summary and review of detailed knowledge of swift
date: 2016-04-29 16:36:21
tags:
---
## The error handling review and conclusion on SWIFT 2 on 2016/04/28

#### 1. Key word Guard

    Like If statement  but for guard, if the statement is true then ELSE is not executed, if Guard statement is false execute the ELSE

    example:
        guard let name = person["name"] else {bala bala bala};

#### 2. Catching and handling error

  2.1 when calling a throwing function which means            it has the key word throw in the function definition, write TRY in front of the function  and the function after TRY might not be executed.

  2.2 use do-catch statement to catch and handle the errors

            do{
                      try ...
                    }catch ...{

                    }

  if catch dose not specify a pattern, the clause will match and bind any error to a local constant named error

  #### 3.Assertions
  If some conditions must be satisfied or the execution must be ended,then use assertions             
  assert( _:_:)  is a global function which takes two arguments             
  First argument:condition.  If the condition is not satisfied then trigger the assert();       
  Second argument: a string to be display at the console. this argument can be omitted    
  example:

            assert(age >= 0 , “a person`s age must be positive integer.”);

  #### 4.Some tips about . and -> in C,C++ and OC
  . operator is used for accessing object member variables and methods via object instance.         
  Foo foo;              
  foo.member_var = 10;
  foo.member_func();            
  -> operator is used for accessing object member variables and methods via pointer.            
  Foo *foo = new Foo();                   
  foo->member_var = 10;                             
  foo->member_func();

  #### 5.closure
  swift syntax:

      { (parameters) -> return type in
                   statements
                 }

  example:

    func authenticate(URLScheme: NSString,callback error:(NSError) -> Void){
       ……
      }

  here second argument error is a closure type, it captures a NSError type and return Void
<!--more-->
  #### 6.singleton:

constructor:
       6.1

    class AppManager {
           private static let _sharedInstance = AppManager()
          class func getSharedInstance() -> AppManager {
          return _sharedInstance
      }
      private init() {}
      }

6.2

    class AppManager {
      static let sharedInstance = AppManager()
      private init() {}
      }

  #### 7.unwind function
           @IBAction func unwind(segue:UIStoryboardSegue){

            }

  #### 8.Multithreading          
       8.1 executing a function on another queue

       let queue:dispatch_queue_t =  dispatch_get_global_queue( DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);//the queue you want to dispatch
       //get the main queue
       let mainQ: dispatch_queue_t = dispatch_get_main_queue();
       let mainQ: NSOperationQueue = NSOperationQueue.mainQueue();//another way to write
       // get the code to execute
       dispatch_async(aQueueWhichIsNotTheMainQueue){
       theFunctionToExecute();//function not to execute on the main Q
            dispatch_async(dispatch_get_main_queue()){
                uiFunctionToUpdateUi();// call ui to show the result above
            }
       }

8.2 some concurrent queues

       QOS_CLASS_USER_INTERACTIVE     //most high priority
       QOS_CLASS_USER_INITIATED       //high
       QOS_CLASS_UTILITY               //long running
       QOS_CLASS_BACKGROUND          //low
       //get the queue
       let dos = Int(<one of the QOS above>.value);
       let queue = dispatch_get_global_queue(qos,0);

#### 9.Property Observers            
       the observers are called every time a property`s value is set.You can add property observer to  any stored property apart from lazy stored property.willSet and didSet are not called in the property initialiser.       

       var steps:Int? {
         willSet(newValue){
             functionToDoWithNewValue();
         }
         didSet(oldValue){
             functionToDoWithOldValue();
         }
       }
       
  ### to be continue
