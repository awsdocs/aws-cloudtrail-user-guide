# Manage event data store lifecycles<a name="query-eds-disable-termination"></a>

The following are the lifecycle stages of an event data store\.
+ `CREATED` – A short\-term state indicating that the event data store has been created\.
+ `ENABLED` – The event data store is active, collecting events, and you can run queries on it\.
+ `PENDING_DELETION` – The event data store has been deleted, but is within the seven\-day wait period before permanent deletion\. You cannot run queries on the event data store, and no operations can be performed on the data store except restoration\.

You can delete an `ENABLED` event data store only if termination protection is disabled\. Termination protection prevents a state change from `ENABLED` to `PENDING_DELETION` to prevent accidental deletion\. By default, termination protection is enabled on an event data store\. After you delete an event data store, it remains in the `PENDING_DELETION` state for seven days before it is permanently deleted\. You can restore an event data store in the `PENDING_DELETION` state back to the `ENABLED` state during the seven\-day wait period\. While in the `PENDING_DELETION` state, an event data store is not available for queries, and no other operations can be performed on the event data store except restore operations\. An event data store in the `PENDING_DELETION` state does not incur costs\.

An event data store in the `PENDING_DELETION` state does not continue to collect logged events\. It also does not count towards your maximum of ten event data stores, unless it is restored\.

To delete or restore an event data store, or turn on or turn off its termination protection, use commands on the **Actions** menu of the event data store's details page\.

![\[Event data store Actions menu\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-eds-actions.png)