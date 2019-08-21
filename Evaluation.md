# Evaluation Instructions

Once you have your development environment setup and you can access the Ignition Gateway Webpage, you're ready to create your demo project and complete the evaluation!  Take a peek at the **[Hints](#Hints)** section below and don't forget to check the [Issues](https://github.com/kcollins-ene/evaluator/issues) section (and feel free to post there if needed) as well.

To get started, here are a few key terms:

| Name                | Description                              |
| ------------------- | ---------------------------------------- |
| Gateway Web Page    | Hosted on Port 8088 of the Ignition Gateway |
| Gateway Status Page | This is the *Status* page off of the *Gateway Web Page* |
| Gateway Home Page   | This is the *Home* page off of the *Gateway Web Page* |
| Gateway Config Page | This is the *Configuration* page off of the *Gateway Web Page* |
| Designer            | Primary Application Development Environment for Ignition |
| Project             | One or more projects can exist on an Ignition Gateway server |
| Client              | Web-Launched Client for a given Ignition Project |

## Setting up Device and Database Connections

Before we actually dive into the creation of a GUI application, we'll get some preliminary items setup.

1. Create/Configure a database connection called `ignition`.  The connection should use MySQL against the `ignition` database with username `ignition` and password `ignition`.
2. Create/Configure two *Generic Simulator* devices, one named `Device1`, another named `Device2`.

The *Gateway Status Page* should now reflect the new database connection and simulated devices:

![Database and Devices Created](images/database_and_devices_created.png)

## Creating an Application

Next, we're going to create a baseline application to house our visualization and tag definitions:

1. Go to the *Gateway Web Page* and select the *Home* section.  Scroll to the bottom and download the *Native Client Launcher* for your platform and use it to connect to your gateway instance and open the Designer.

2. Create a project called `Evaluator` using one of the *Single-Tier* navigation templates that will provide us with menuing and navigation preconfigured.

3. Adjust the project properties to utilize *Anchored* mode as the default component layout methodology.

Lets setup some framework elements for our evaluation:

1. Open the `Navigation` window from the *Project Browser*.

2. Remove the Ignition logo element and replace with the Evaluator Logo image:
   ![Evaluator Logo](images/evaluator_logo.png)

3. Use the *Tab Strip Customizer* on the `Tabs` component to add a new tab called `Demo` for a demo window we'll create soon.

   > It should be placed in between the `Overview` and `User Management` tabs.

4. Save and close the `Navigation` window.

We need some tags in order to drive our displays, so lets create some:

1. Bring up the OPC Browser against the internal `Ignition OPC-UA Server`.  Add the entire `Devices` folder to the tags in the default provider for the project by just dragging the folder onto the `Tags` folder in the *Tag Browser*.

   > Note: The bolded *Tags* folder in the *Tag Browser* represents the tag provider that has been configured as *Default* for this project.  You can see all of the providers (including the project default) in the *All Providers* folder.

2. Adjust the `Default Historical` scan class to utilize a faster *Slow Rate* of `1000`ms so our history capture will be a little speedier.

3. Enable History on the `Realistic/Realistic0` and `Writable/WriteableDouble1` tags for both `Device1` and `Device2`. 

   > Don't forget to set the history provider!

4. Modify the metadata on `Writeable/WriteableDouble1` on each device to allow for Engineering Units range from `-100` to `100`.

5. Enable a `Hi Alarm` on `Realistic/Realistic0` that activates when its value exceeds `15` for `Device1` and `20` for `Device2`

Now that we have some data, lets get started with our `Demo` window:

1. Create a new *Main Window* in the `Main Windows` folder called `Demo`.

2. Add an *Easy Chart* component to the screen.  

3. Configure the *Easy Chart* you created to have 2 subplots.  One of the subplots should have pens for the `Realistic/Realistic0` and `Writeable/WriteableDouble1` tags from `Device1`, the other subplot should have the same from `Device2`.

   > Use a dark gray line for the `Realistic/Realistic0` values and a colored line (of your choosing) for the `Writeable/WriteableDouble1` tags.
   >
   > While you can edit the datasets of the *Easy Chart* manually, feel free to use the *Easy Chart Customizer* feature to make these additions.

4. Also configure the *Easy Chart* to utilize *Realtime* mode.  Disable *Pen Control?* to hide the configured pens listing.

5. Let's also add *Numeric Label* components so we can visualize the `Realistic/Realistic0` and `Writeable/WriteableDouble1` tags from `Device1` and `Device2`.  Name the components and lay them out in a reasonable manner of your choosing.  

6. Modify the *Numeric Label* components you created to have the *Background Color* change to orange when the `AlertActive` property is true on `Realistic/Realistic0` (for each respective device). 

7. For the benefit of our users, lets also add some *Label* components to provide a description to the left of those *Numeric Label* components we added above.  Set the text on the labels to `Value` and `Snapshot` for the `Realistic/Realistic0` and `Writeable/WriteableDouble1` tags, respectively.

   > You can also leverage *Container* components to group other components together.  Try experimenting with the border styles of the *Container* components to add decorative groupings to your application!

8. Finally, add a button (for each of the value pairs we created above) that will set the `Writeable/WriteableDouble1` tag with the value of `Realistic/Realistic0` from each device.  Set the button text to indicate that it will snapshot the associated device.

   > Once you configure the button for the *Set Tag Value* action, take a look at the *Script Editor* tab to see what it created.  You might see if you can leverage the `system.tag.read()` function to read the value directly from the tag instead of from a property.

9. Work with the layout features of the components you've created to make this window function properly on any size screen (prioritize giving the most real estate to the trend).  Consult the the documentation here for more information on layout: [Component Layout](https://docs.inductiveautomation.com/display/DOC79)

10. Save and close your `Demo` window.  

11. Save and Publish the application.  Then try previewing the project via the Designer's *Tools->Launch Project* menu.

Once complete, you should have a window that will allow viewing some of the simulated values as well as the snapshot values (which should change when you click the snapshot button you created).

## Hints

Here are a few helpful Ignition-related hints to help you get started:

* Within the *Property Editor*, by default, you can only see *Basic* properties.  Utilize the *Filter properties* selector to expand your view to *All* properties to reveal all possible configuration options.
* You can drag tags from the *Tag Browser* to the chart components to add tags as an easier method to finding them from within the customizer.
* You can drag container components around on your window if you hold down the *Alt* key while dragging.
* Keep in mind that there is a 2-hour trial that can be reset easily from the Gateway webpage.  If you find that your history values are not being captured in the Ignition Designer, this is probably the reason.

## Submitting your Project

When you're satisfied with the state of your project, utilize the *Gateway Config Page* under the *System->Backup/Restore* section to download a Gateway Backup.  Post to your favorite cloud service and send a link to your ENE representative for evaluation!

Thank you for your efforts and we hope you enjoyed this small introduction to one of the many platforms you'll experience as an SI!
