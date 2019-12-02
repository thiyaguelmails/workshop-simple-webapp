# Getting Started with RDS

Task 1: Create a Security Group for the RDS DB Instance
-------------------------------------------------------

In this task, you will create a security group to allow your web server
to access your RDS DB instance. The security group will be used when you
launch the database instance.

5.  In the **AWS Management Console**, on the []{#anchor}Services menu,
    click **VPC**.
6.  In the left navigation pane, click **Security Groups**.
7.  Click []{#anchor-1}Create security group and then configure:

    -   **Security group name:** *DB Security Group*
    -   **Description:** *Permit access from Web Security Group*
    -   **VPC:** *Lab VPC*

8.  Click []{#anchor-2}Create then click []{#anchor-3}Close

    You will now add a rule to the security group to permit inbound
    database requests.

9.  Select **DB Security Group**.
10. Click the **Inbound Rules** tab.

    The security group currently has no rules. You will add a rule to
    permit access from the **Security Group you made in module 1**.

11. Click []{#anchor-4}Edit rules
12. Click []{#anchor-5}Add Rule then configure:

    -   **Type:** *MySQL/Aurora (3306)*
    -   **CIDR, IP, Security Group or Prefix List:** Type *sg* and then
        select *Web Security Group*.

    This configures the Database security group to permit inbound
    traffic on port 3306 from any EC2 instance that is associated with
    the *Web Security Group*.

13. Click []{#anchor-6}Save rules then click []{#anchor-7}Close

    You will use this security group when launching the Amazon RDS
    database.

 

Task 2: Create a DB Subnet Group
--------------------------------

In this task, you will create a *DB subnet group* that is used to tell
RDS which subnets can be used for the database. Each DB subnet group
requires subnets in at least two Availability Zones.

14. On the []{#anchor-8}Services menu, click **RDS**.
15. In the left navigation pane, click **Subnet groups**.

    If the navigation pane is not visible, click the menu icon in the
    top-left corner.

16. Click []{#anchor-9}Create DB Subnet Group then configure:

    -   **Name:** *DB Subnet Group*
    -   **Description:** *DB Subnet Group*
    -   **VPC:** *Lab VPC*
    -   **Availability zone:** Select the *first* Availability Zone
    -   **Subnet:** *10.0.1.0/24*
    -   Click []{#anchor-10}Add subnet

    This added Private Subnet 1. You will now add Private Subnet 2.

17. Configure these settings (on the existing screen):

    -   **Availability zone:** Select the *second* Availability Zone
    -   **Subnet:** *10.0.3.0/24*
    -   Click []{#anchor-11}Add subnet

    These subnets should now be shown in the list: **10.0.1.0/24** and
    **10.0.3.0/24**

18. Click []{#anchor-12}Create

    You will use this DB subnet group when creating the database in the
    next task.

 

Task 3: Create an Amazon RDS DB Instance
----------------------------------------

In this task, you will configure and launch a Multi-AZ Amazon RDS for
MySQL database instance.

Amazon RDS *****Multi-AZ***** deployments provide enhanced availability
and durability for Database (DB) instances, making them a natural fit
for production database workloads. When you provision a Multi-AZ DB
instance, Amazon RDS automatically creates a primary DB instance and
synchronously replicates the data to a standby instance in a different
Availability Zone (AZ).

19. In the left navigation pane, click **Databases**.
20. Click []{#anchor-13}Create database

    If you see **Switch to the new database creation flow** at the top
    of the screen, please click it.

21. Select **MySQL**.
22. Under **Settings**, configure:

    -   **DB instance identifier:** *lab-db*
    -   **Master username:** *master*
    -   **Master password:** *lab-password*
    -   **Confirm password:** *lab-password*

23. Under **DB instance size**, configure:

    -   Select **Burstable classes (includes t classes)**.
    -   Select *db.t3.micro*

24. Under **Storage**, configure:

    -   **Storage type:** *General Purpose (SSD)*
    -   **Allocated storage:** *20*

25. Under **Connectivity**, configure:

    -   **Virtual Private Cloud (VPC):** *Lab VPC*

26. Expand **Additional connectivity configuration**, then configure:

    -   For **Existing VPC security groups:** click *DB Security Group*
        to highlight it in blue.

27. Expand **Additional configuration**, then configure:

    -   **Initial database name:** *lab*
    -   Uncheck **Enable automatic backups**.
    -   Uncheck **Enable Enhanced monitoring**.

    This will turn off backups, which is not normally recommended, but
    will make the database deploy faster for this lab.

28. Click []{#anchor-14}Create database

    Your database will now be launched.

    If you receive an error that mentions "not authorized to perform:
    iam:CreateRole", make sure you unchecked *Enable Enhanced
    monitoring* in the previous step.

29. Click **lab-db** (click the link itself).

    You will now need to wait **approximately 4 minutes** for the
    database to be available. The deployment process is deploying a
    database in two different Availability zones.

    While you are waiting, you might want to review the [Amazon RDS
    FAQs](https://aws.amazon.com/rds/faqs/) or grab a cup of coffee.

30. Wait until **Info** changes to **Modifying** or **Available**.
31. Scroll down to the **Connectivity & security** section and copy the
    **Endpoint** field.

    It will look similar to:
    *lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com*

32. Paste the Endpoint value into a text editor. You will use it later
    in the lab.

 

Task 4: Interact with Your Database
-----------------------------------

In this task, you will open a web application running on your web server
and configure it to use the database.

33. To copy the **WebServer** IP address, click on the
    []{#anchor-15}Details drop down menu above these instructions, and
    then click []{#anchor-16}Show.
34. Open a new web browser tab, paste the *WebServer* IP address and
    press Enter.

    The web application will be displayed, showing information about the
    EC2 instance.

35. Click the **RDS** link at the top of the page.

    You will now configure the application to connect to your database.

36. Configure the following settings:

    -   **Endpoint:** Paste the Endpoint you copied to a text editor
        earlier
    -   **Database:** *lab*
    -   **Username:** *master*
    -   **Password:** *lab-password*
    -   Click **Submit**

    A message will appear explaining that the application is executing a
    command to copy information to the database. After a few seconds the
    application will display an **Address Book**.

    The Address Book application is using the RDS database to store
    information.

37. Test the web application by adding, editing and removing contacts.

    The data is being persisted to the database and is automatically
    replicating to the second Availability Zone.


