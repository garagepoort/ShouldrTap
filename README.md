ShouldrTap
==========

ShouldrTap is our own implementation of the observer pattern where the update method is specific for a certain event.


Why necessary?
==============

The Java Observer pattern has some flaws. One of its major flaws is that the update method just has an Object as parameter. There is no way to specify what kind of object should be past to the update method. This results in the use of instanceof.  Also if many different classes are observing the same class it is not always the case that one notification is meant for everyone. So a lot of observing classes execute there update method without doing anything in them except for checking if they have to do something based on the class of the object being passed.


Class explanation:
=================


We tried to make a library that uses specific events and event handlers.
Class overview

    Shoulder
    Tapper
    AbstractEvent

Shoulder
========

Shoulder can be considered as the event handler.  It uses generics to determine what kind of event it can handle.  When extending the Shoulder class, the update(T event) method should be overwritten. The constructor should also be implemented and the argument given to super() should be the classtype of the event this shoulder can handle.

```java
public class ErrorShoulder extends Shoulder<ErrorEvent>
{
    private Activity activity;


	public ErrorShoulder(Activity activity)
	{
		super(ErrorEvent.class);
		this.activity = activity;
	}


	@Override
	public void update(ErrorEvent event)
	{
		System.out.println(event.getData().getMessage());
		DialogCreator.showErrorDialog(event.getData().getMessage(), activity);
	}

}
```

Tapper
======
The tapper class can be compared to the Observable class in Java. It contains a list of shoulders. Shoulders can be registered or unregistered to this tapper. The tapper has a method “tapShoulders(AbstractEvent event)” . Calling this method notifies all the shoulders that can handle the event passed to the tapShoulders method. (that are registered to this tapper)

```java
public class ContactsModel extends Tapper {

	private static final ContactsModel INSTANCE = new ContactsModel();
	private List<Contact> contacts = new ArrayList<Contact>();

	public static ContactsModel getInstance() {
		return INSTANCE;
	}

	private ContactsModel() {
	}

	public void setContacts(List<Contact> contacts) {
		this.contacts = contacts;
		ContactListUpdatedEvent event = new ContactListUpdatedEvent();
		event.setData(contacts);
		tapShoulders(event);
	}

	public List<Contact> getContacts() {
		return Collections.unmodifiableList(contacts);
	}

	public void addContact(Contact contact) {
		if (!contacts.contains(contact)) {
			contacts.add(contact);
			ContactListUpdatedEvent event = new ContactListUpdatedEvent();
			event.setData(contacts);
			tapShoulders(event);
		}
	}
}
```

AbstractEvent
=============
The abstract event is just simply a class you should extends if you want to make your own ustom events.

```java
public class ErrorEvent extends AbstractEvent<Exception>
{
	private Exception exception;


	@Override
	public Exception getData()
	{
		return exception;
	}


	@Override
	public void setData(Exception exception)
	{
		this.exception = exception;
	}

}
```
