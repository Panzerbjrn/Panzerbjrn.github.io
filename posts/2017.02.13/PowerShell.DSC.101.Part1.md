Or: PowerShell DSC For Dummies...

When I started leaning about PowerShell DSC, I found lots of great technical information, but nothing to really describe the key concepts and ideas that made it work, and how they fit together. I am here going to try to document some of these and hopefully this will help you on your journey to PowerShell DSC nirvana.

I am going to make the following assumptions:

* That you know PowerShell reasonably well
* That you have looked at PowerShell DSC and have an idea what it is for
* That you're an IT pro in some capacity
* You are using WMF5 or later

So, a couple of concepts to start out with, that I think would be useful to have clearly explained before you get started.

Pull Server - This is the server that hosts the MOF files that are used to configure clients/nodes.


Push Server - A server that pushes out the MOF files


Pull Client - The machine/device that pulls a configuration from a Pull Server. Also called a Node.


Node - Same as a Pull/Push Client.


Push Mode - The mode that your DSC configuration is in, i.e. the configuration is pushed to the clients.


Pull Mode - The mode that your DSC configuration is in, i.e. the configurations are pulled from a central server by the clients.




That's the devices/modes explained; but what is actually used to tell Powershell DSC how/what to configure:


Configuration Keyword - This is similar to the "Function" keyword in that it tells PowerShell that you are creating a configuration rather than running a PowerShell script (Or a Function if you had started the block with the keyword Function). You put this at the start of your configuration.


Configuration - This is the collection of information that tells PowerShell DSC how nodes specified in the ConfigurationData should be configured. I.e. which windows features should be installed, How File Shares should be configured, How IIS should be set and so on.


ConfigurationData - This is, basically, a list of the nodes that PowerShell DSC will configure as per the Configuration.


MOF File - Management Object Format - This is the file that is generated when you run a Configuration against the ConfigurationData (i.e. when you tell DSC what to do and which nodes to do it to).Clients will use this file to identify what their settings should be.


Resources - These are what tells PowerShell DSC how to do what was specified in the MOF file. They are not unlike CMDlets, except that they are run by the DSC Configuration Manager, rather than called by an admin sitting at a keyboard.


LocalConfigurationManager - The LCM is the bit in PowerShell that applies the settings specified in the MOF.


In conclusion:


You create your Configuration; you run the Configuration against your ConfigurationData; which creates a MOF file; your Pull Server hosts the MOF file; your Clients/Nodes connects and reads the MOF; the Node's LocalConfigurationManager (LCM) applies the settings in the MOF using the Resources needed.

















































