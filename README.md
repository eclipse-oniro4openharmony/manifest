# Oniro Project Manifest Repository

Welcome to the Oniro Project's manifest repository, a fork of the OpenHarmony manifest with enhancements tailored for the Oniro ecosystem.

## 1. Repository Classification

To streamline code fetching for various system profiles, Oniro (inherited from OpenHarmony) categorizes each repository using groups:

| Category           | Description                                                          | Group          |
| ----------------- | -------------------------------------------------------------------- | -------------- |
| **Mini System**   | Repositories for lightweight system builds                           | ohos:mini      |
| **Small System**  | Repositories for small system targets                                | ohos:small     |
| **Standard System**| Repositories for full-featured standard system builds               | ohos:standard  |
| System Components | Hardware-agnostic system components deployed under `/system`        | ohos:system    |
| Chipset Components| Hardware-specific components deployed under `/vendor` or `/chipset` | ohos:chipset   |

Each repository must be part of the `default` group, and can belong to multiple groups using a comma-separated list, e.g.:

```xml
<project name="inputmethod_imf" path="base/miscservices/inputmethod" groups="ohos:standard,ohos:system"/>
<project name="ai_engine" path="foundation/ai/ai_engine" groups="ohos:mini,ohos:small,ohos:standard,ohos:system"/>
```

Repositories targeting the standard system must also specify either `ohos:system` or `ohos:chipset`.

## 2. Platform and Chipset Repositories

Oniro supports a growing number of chipset platforms. Platform-independent repositories are grouped under the **manifests/ohos/ohos.xml**, while chipset-specific ones are under **manifests/chipsets/**.

In addition, Oniro defines a dedicated manifest file, **oniro.xml**, which represents the Oniro-specific distribution and includes both upstream and Oniro-specific repositories.

Repository layout:

```
manifest/
├── default.xml
├── ohos
│   └── ohos.xml
├── oniro.xml
└── chipsets/
    ├── all.xml
    ├── chipset1.xml
    ├── chipset2.xml
    ├── chipsetN.xml
    ├── chipset1/chipset1-detail.xml
    ├── chipset2/chipset2-detail.xml
    └── chipsetN/chipsetN-detail.xml
```

- `default.xml`: Combines `ohos/ohos.xml` and `chipsets/all.xml` — fetches all platform and chipset repositories.
- `oniro.xml`: Defines the Oniro-specific distribution manifest.
- `chipsets/chipsetN.xml`: Combines `ohos/ohos.xml` with the chipset's detail file to fetch code for a specific chipset.
- `chipsets/all.xml`: Aggregates all chipset-specific detail files.

> Note:
> Multiple development boards may share `device/soc` or `vendor` repos. Avoid duplicate entries in `chipsets/all.xml` when boards share common components.

## 3. Repo Initialization and Code Fetching

To support global developers, Oniro mirrors the source on its own infrastructure while falling back to [GitCode](https://gitcode.com/openharmony/manifest) as needed. Below are example commands for various scenarios:

| **System Type** | **Scope**          | **Command**                                                                 | **Description**                                  |
|-----------------|--------------------|-----------------------------------------------------------------------------|--------------------------------------------------|
| All             | All code           | `repo init -u <URL> -b master`                                              | Fetches all Oniro repositories.                  |
| Mini            | All                | `repo init -u <URL> -b master -g ohos:mini`                                 | Fetches all mini system repositories.            |
| Mini            | Specific chipset   | `repo init -u <URL> -b master -m chipsets/chipsetN.xml -g ohos:mini`        | Mini system for a specific chipset.              |
| Small           | All                | `repo init -u <URL> -b master -g ohos:small`                                | Fetches all small system repositories.           |
| Small           | Specific chipset   | `repo init -u <URL> -b master -m chipsets/chipsetN.xml -g ohos:small`       | Small system for a specific chipset.             |
| Standard        | All                | `repo init -u <URL> -b master -g ohos:standard`                             | Fetches all standard system repositories.        |
| Standard        | Specific chipset   | `repo init -u <URL> -b master -m chipsets/chipsetN.xml -g ohos:standard`    | Standard system for a specific chipset.          |
| Standard        | System components  | `repo init -u <URL> -b master -g ohos:system`                               | Only system components.                          |
| Standard        | Chipset components | `repo init -u <URL> -b master -g ohos:chipset`                              | All chipset-related components.                  |
| Standard        | Specific chipset components | `repo init -u <URL> -b master -m chipsets/chipsetN.xml -g ohos:chipset` | Specific chipset components only.                |

Replace `<URL>` with the [Oniro manifest](https://github.com/eclipse-oniro4openharmony/manifest.git).

## 4. `matrix_product.csv`

The [`matrix_product.csv`](https://gitee.com/openharmony/manifest/blob/master/matrix_product.csv) file defines supported build types for each repository and the corresponding test cases. Each row maps a repo to build profiles and test configurations.

- Column A: Repository name
- Column B: Component name
- Columns C onward: Build profile (`Y`/`N`) and test suites (`Test_{profile}`)

### Updating or Adding Repositories
Add a new row for each new repository. Ensure the correct build and test profiles are filled in.

---

For further documentation and source acquisition guides, see: [Oniro Docs](https://docs.oniroproject.org/)

