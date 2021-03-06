---
title: "A Word on Security"
slug: security-parse
---

Security is a very important topic - so we should discuss it as early as possible. Luckily, when using Parse, we don't need to spend too much time thinking about security features, as the login mechanism (including password storage) are all built-in.

There is one aspect that we need to take care of - _Access Control Lists_.

#An Introduction to Access Control Lists

_Access Control Lists_ (ACL) allow you to define lists of users that have certain rights to read, modify, or delete certain objects. The default ACL is _Public Read and Write_. This means any user can read this object, modify it and even delete it.

The current ACL for each object can be found by looking in the column labeled *ACL* in the Parse data browser.

![Parse database row](public_read_write.png)

For most objects you store in Parse **you don't want this default ACL.** If any of your users gets access to your _ClientKey_ (which is possible in multiple different ways) they will be able to modify all objects that have public write access!

If you store all of your objects with the default ACL, it would allow an attacker to delete all of your app's data!

How can we avoid that? By changing the default ACL!

#Changing the Default ACL

Parse provides an API call that allows us to change the default ACL for all objects created in the app. We will use that to change the default to a security setting that:

1. Allows public _read_ access - any user can see all objects created with this default ACL
2. Only provides _write_ access to the user that created the object

We will set up this default ACL directly after app launch, within our `AppDelegate`.

> [action]
Extend the `application(_:didFinishLaunchingWithOptions:)` method, in the `AppDelegate` class, by adding the three lines of code that change the default ACL:
>
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
>
        //...
>
        let acl = PFACL()
        acl.publicReadAccess = true
        PFACL.setDefaultACL(acl, withAccessForCurrentUser: true)
>
        return true
    }

Now every new Parse object will be equipped with public read access and write access that is limited to the object's creator.

If you upload a new photo to Parse, you should now see a different entry in the ACL column:

![Parse row in database](public_read_user_write.png)

With this new default setting our app content is already much safer!

#Cleaning up the Old Data

Thus far in the **Makestagram** tutorial, we have created multiple posts. Most of these posts are invalid as they either don't have a user assigned to them or they have the wrong ACL. To avoid debugging issues related to outdated objects, let's delete all of the existing posts.

> [action]
Use the Parse data browser to delete all rows of the `Post` class:
>
1. Check all the check boxes for each row
1. Click `Edit`, select `Delete these rows`, and confirm
>
![Deleting all the Post rows in the Parse Dashboard](delete_posts.png)

#Conclusion

This step gave you a basic overview of ACLs - an important security feature in Parse.

You also learned how to change the default ACL. Changing the default ACL should be one of the first steps when creating your own apps using Parse.

If you are interested in some more details on ACLs, you can read the official documentation [here](https://parse.com/docs/ios/guide#security-object-level-access-control).

For now this step concludes our solution for creating new posts. In the next step we will start implementing one of the core features of **Makestagram**: the timeline!
