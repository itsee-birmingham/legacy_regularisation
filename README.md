# legacy_regularisation

The code in this repository provides legacy support for projects generated using versions of collation_editor_core prior to release 1.1.0. In that release the way that regularisation rules were changed significantly due to a bug identified in the chaining of rules. Projects which generated their regularisation rules in earlier versions may find that rules that were previously not applied will be applied. This will only happen where rules have been chained together in quite specific ways but it may still be worth preserving the older system for such projects.

## To provide this legacy support

To include this in collation_editor_core first clone the collation_editor_core code. Then inside that directory add this repository by downloading the zip file and unziping so that you have the legacy_conversion folder directly in the cloned respository of the core code. Please do not commit this folder to the core repository on github it is optional which is why it should not be added as a git submodule.

There are multiple ways that this code could be used in your installation. The way I have chosed to do this is to host two different collation urls: one for the latest regularisation and one for the legacy regularisation. In the Django services file in the doCollate I check the name of the project at CL.project.name and if it is in my list of legacy projects it gets sent to the legacy url and otherwise it gets sent to the normal url. 
