# 💭 S3 Bucket Lifecycle Configuration

1. In the **Buckets** list, choose the name of the bucket that you want to create a lifecycle rule for.
2. Choose the **Management** tab, and choose the **Create lifecycle rule**.
3. In the **Lifecycle rule name**, enter a name for your rule.

> [!NOTE]
>
> The name must be unique within the bucket.

4. Choose the scope of the lifecycle rule:

    a. To apply this lifecycle rule to *all obects with a specific prefix or tag*, choose **Limit the scope to specific prefixes or tags**.
      * To limit scope by prefix, in **Prefix**, enter the prefix.
      * To limit the scope by tag, choose **Add tag**, and enter the tag key and value. 
      * To filter a rule by object size, you can check **Specify minimum object size**, **Specify maximum object size**, or both options.
        * When you are specifying a **minimum object** or **maximum object** size, the alue must be larger than `0 bytes` and up to `5TB`. You can specify this value in bytes, KB, MB or GB.
        * When you are specifying both, the maximum object size must be larger than the minimum object size.
      * To apply this lifecycle rule to *all objects in the bucket*, choose **This rule applies to all objects in the bucket**, and choose **I acknowledge that this rule applies to all objects in the bucket**. 

5. Under **Lifecycle rule actions**, choose the actions that you want your lifecycle rule to perform.

    * Transition current versions of objects between storage classes.
    * Transition previous versions of objects between storage classes.
    * Expire current versions of objects.
    * Permanently delete previous versions of objects.
    * Delete expired delete markers or incomplete multipart uploads.

6. To *transition current versions of objects* between storage classes, under **Transition current versions of objects between storage classes**:

    * In **Storage class transitions**, choose the storage class to transition to:
      * Standard-IA
      * Intelligent-Tiering
      * One Zone-IA
      * S3 Glacier Flexible Retrieval
      * Glacier Deep Archive
    * In **Days after object creation**, enter the number of days after creation to transition the object.

## 📋 Related Articles
- [Setting a lifecycle configuration on a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-set-lifecycle-configuration-intro.html)