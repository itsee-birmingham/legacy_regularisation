# Introduction

The code in this repository provides legacy support for projects generated using versions of collation_editor_core prior to release 1.1.0. In that release the way that regularisation rules were changed significantly due to a bug identified in the chaining of rules. Projects which generated their regularisation rules in earlier versions may find that rules that were previously not applied will be applied. This will only happen where rules have been chained together in quite specific ways but it may still be worth preserving the older system for such projects.

# Referencing

To cite the collation editor core code please use the doi:
[![DOI](https://zenodo.org/badge/303814336.svg)](https://zenodo.org/badge/latestdoi/303814336)

# To provide this legacy support

To include this in collation_editor_core first clone the collation_editor_core code. Then within the same parent folder clone this repository so it ends up as a sibling of the core code.

There are multiple ways that this code could be used in your installation. The way I have chosed to do this is to host two different collation urls: one for the latest regularisation and one for the legacy regularisation. In the Django services file in the doCollate I check the name of the project at CL.project.name and if it is in my list of legacy projects it gets sent to the legacy url and otherwise it gets sent to the normal url.

# Additions required to services file and python support

- #### ```localPythonImplementations```

**This variable can be overwritten in individual project settings**

This variable takes for form of a JSON object.

- **prepare_t** should be provided if any processing was done in the extraction of the token JSON in  
  order to create the t value. This method should do the same as that process. It is used to determine if rules should be applied before or after the data is sent to collateX. For example, if when preparing the JSON data, you lowercase the value of t then collateX will be sent the lowercase version and therefore any rules which change the case should be applied to the data once it is returned from collateX.

  This method is provided with:
  - a string representing the value of the current word
  - a dictionary of the display settings with the id of each setting providing the key to a boolean that describes whether it is selected or not
  - an array containing the JSON objects of the full display_settings configuration.

  It should return the string modified to the same way as the t values in the token JSON data is created.


An example of the config is shown below:

```js
localPythonImplementations = {
    "prepare_t": {
        "python_file": "collation.greek_implementations",
        "class_name": "PrepareData",
        "function": "prepare_t"
    }
};
```

The corresponding python for the config above is below:

```python
class PrepareData(object):

    def prepare_t(self, string, display_settings={}, display_settings_config=[]):
        #turn it into a dictionary so we can use other functions
        settingsApplier = ApplySettings()
        token = {'interface': string}
        token = settingsApplier.lower_case_greek(token)
        token = settingsApplier.hide_supplied_text(token)
        token = settingsApplier.hide_unclear_text(token)
        token = settingsApplier.hide_apostrophes(token)
        token = settingsApplier.hide_diaeresis(token)
        return token['interface']

```
