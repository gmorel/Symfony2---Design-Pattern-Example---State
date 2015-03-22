[![Build Status](https://travis-ci.org/gmorel/StateWorkflowBundle.svg?branch=master)](https://travis-ci.org/FriendsOfSymfony/FOSRestBundle)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/gmorel/StateWorkflowBundle/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/FriendsOfSymfony/FOSRestBundle/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/gmorel/StateWorkflowBundle/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/FriendsOfSymfony/FOSRestBundle/?branch=master)
[![Total Downloads](https://poser.pugx.org/gmorel/StateWorkflowBundle/downloads.svg)](https://packagist.org/packages/FriendsOfSymfony/rest-bundle)
[![Latest Stable Version](https://poser.pugx.org/gmorel/StateWorkflowBundle/v/stable.svg)](https://packagist.org/packages/FriendsOfSymfony/rest-bundle)

State Workflow Bundle
=====================

State Design Pattern implementation for a Symfony2 project
---------------------------------------------

Keywords : State Machine, State Design Pattern, Workflow, Finite State Machine, Finite State Automaton

# WIP

Helping you managing a complex yet easily maintainable workflow.

![Workflow](https://raw.githubusercontent.com/gmorel/StateWorkflowBundle/master/doc/booking-wokflow.png "Workflow")

A StateWorkflow object is responsible for managing all States and Transitions for a given Entity.
Every single State is a Class implementing StateInterface and is managing its own transition.

### Ubiquitous Language
- State : an Entity state at a given time (ex: `Booking payed`, `Quote cancelled`, etc..)
- Transition : a transition between state A and state B (ex: `Booking waiting for payment` --`Send confirmation mail`-> `Booking payed`, etc..)


### Pros
- All your workflow described via classes
- All your workflow can be easily  **Unit Tested**
- Workflow States can be modified **dynamically** at run time
- Each State is responsible from **its own transitions**
- Entity's current state can be **easily stored** in database (simple string)
- Workflow **specification doc can be generated from code**


### Cons
- Each time you add a transition you have to modify your own interface extending `StateInterface` implementation
- Can be too complex for too small workflow (ie. less than different 3 states)
- Not really follow the famous precept : prefer composition over inheritance..



### Usage

```php

$bookingStateWorkflow = $this->get('demo.booking_engine.state_workflow');

// Initialize entity state to booking workflow default state : incomplete
// Booking __construct contains 
// $bookingStateWorkflow->getDefaultState()->initialize($this);
$booking = new Booking($bookingStateWorkflow, 200);

// Set incomplete entity as paid
// Take care of the state transition (incomplete -> paid)
// Send confirmation mail
$booking->getState($bookingStateWorkflow)->setBookingAsPaid($booking);

// Get current booking state : StatePaid
$currentState = $booking->getState($bookingStateWorkflow);

```

### Details

It will allow you to manage `States` and especially their available `Transitions` for an `Entity` (for example a Booking class) implementing our ```HasStateInterface```.
It is aiming at helping implementing a complex `Workflow` where each `State` implementing our ```StateInterface``` is responsible for its `Transitions` (methods) to other `States`.
Some `Transitions` being impossible (not part of your `Workflow`) and then throwing the ```UnsupportedStateActionException``` exception whenever called.

Each `State` is a *service* tag (ex: ```demo.booking_engine.state```).
This way you will be able to manage available `States` for different `Entities` by using other *tags* (ex: Booking, Content and Customer shall not share the same `Workflow`/`States`). 
You will then need to modify the CompilerPass in order to let your `Workflow` be aware of its `States`.

![UML](https://raw.githubusercontent.com/gmorel/StateWorkflowBundle/master/doc/uml-statebundle-simplified.png "UML")


#### Adding new State:

In case you wish to add a new `State` you will need to create a new Class implementing our ```StateInterface``` interface.

#### Adding new Transition:

In case you wish to add a new `Transition` you will need to add a new method in your ```XXXStateInterface``` extending our ```StateInterface```. 
You can also use our helper ```AbstractState``` to implement default behavior ie. a method throwing our `UnsupportedStateTransitionException`.


### Credits

Feel free to enhance it and to share your ideas/enhancements.

Licence: MIT
