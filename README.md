# legacy_regularisation

The code in this repository provides legacy support for projects generated using versions of collation_editor_core prior to release 1.1.0. In that release the way that regularisation rules were changed significantly due to a bug identified in the chaining of rules. Projects which generated their regularisation rules in earlier versions may find that rules that were previously not applied will be applied. This will only happen where rules have been chained together in quite specific ways but it may still be worth preserving the older system for such projects.

## To provide this legacy support

To include this in collation_editor_core first clone the collation_editor_core code. The inside that directory add this repository as a submodule as follows:

``` git submodule add https://github.com/itsee-birmingham/legacy_regularisation.git legacy_regularisation ```

