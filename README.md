This repo supplies extra metainfo for appstream which are not in a package.
It should only be used for "fake" components such as operating system defintions
or to remove components which are in ubuntu but we do not want to show up.

Below is a verbatim copy of the relevant documentation. The current should
be at https://github.com/ximion/appstream-generator/blob/master/docs/asgen-config.md

**When marking things for removal do try to mention "why" in the associated .md file!**

## Injecting extra metainfo / removing components

Sometimes injecting metainfo files directly into the generation process instead of packaging them makes sense. This can be done for example for `web-application` components, `operating-system` components or for components which are merged into others. It is discouraged to use this feature for any components that are directly tied to a package.
Metainfo XML files can be placed in `%ExtraMetainfoDir%/suite/section/(arch)`, where `%ExtraMetainfoDir%` usually is the `extra-metainfo` directory in the generator's current workspace, unless this default is overriden in the configuration file.
If the `arch` part of the path is omitted, a metainfo file is added to all active architectures. E.g. a XML file placed in `%ExtraMetainfoDir%/buster/main/` will apply to all active architectures for `buster`, while files in `%ExtraMetainfoDir%/buster/main/amd64/` will only apply to the `amd64` architecture.

Additionally to metainfo XML files, a `removed-components.json` file may be placed in the extra-metainfo directories. This JSON file contains a list of component IDs that should be removed from the metadata pool on client machines. By listing IDs in this file, the generator will create a special component that will trigger a component with the same ID to be removed from the metadata pool if its priority is lower.
This feature is useful for overlay suites, for example if the `buster-updates` suite (with priority 10) wants to completely remove a component from the `buster` suite (with priority 0) from the user's eyes. This may be necessary if a component improperly changes its ID, if you are maintaining an overlay distribution and can not control the composition of packages in the base suites, or in case of legal issues where a component has to be removed retroactively from a frozen distribution suite. It may also be useful to hide `web-application` components in a stable distribution in case their service is discontinued before the distribution release is out of support as well.

Internally, injected metainfo data is grouped under a special fake package name, in order to allow the generator to properly record hints for the added components and to associate the data with the right suite(s).
