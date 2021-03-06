---
title: Creating a Grasshopper Plug-In Package
description: This is a step by step guide to creating a package for a Grasshopper plug-in (.gha).
authors: ['will_pearson']
sdk: ['Yak']
languages: # empty
platforms: ['Windows', 'Mac']
categories: ['Getting Started']
origin: unset
order: 10
keywords: ['developer', 'yak']
layout: toc-guide-page
---

The [Package Manager](../yak/) is a new feature in Rhino 7 WIP. It makes it easier to discover, install and manage Grasshopper plug-ins from within Rhino. This guide will describe how to create a package from a Grasshopper plug-in that can be published to the package server.

<div class="alert alert-info" role="alert">
<strong>Note:</strong> The package manager is cross-platform. The examples below are for Windows.
For Mac, replace the path to the Yak CLI tool with
<code>"{{ site.rhino.mac_path }}/Contents/Resources/bin/yak"</code>.
</div>

{% include yak-mac-path-note.html %}

First, let's assume you have a folder on your computer which contains all the
files that you would like to distribute in your package. Something like this...

```commandline
C:\Users\Bozo\dist
├── Marmoset.gha
├── Marmoset.dll
└── misc\
    ├── README.md
    └── LICENSE.txt
```

We're going to use the Yak CLI tool to create the package, so open up a Command
Prompt and navigate to the directory above.

```commandline
> cd C:\Users\Bozo\dist
```

Now, we need a `manifest.yml` file! You can easily create your own by studying
the [Manifest Reference Guide](../the-package-manifest). Alternatively, you can use the `spec`
command to generate a skeleton file. We'll do the latter here.

```commandline
> "{{ site.rhino.windows_path }}\System\Yak.exe" spec

Inspecting content: Marmoset.gha

---
name: marmoset
version: 1.0.0
authors:
- Park Ranger
description: >
  This plug-in does something. I'm not really sure exactly what it's supposed to
  do, but it does it better than any other plug-in.
url: https://example.com


Saved to C:\Users\Bozo\dist\manifest.yml
```

The `spec` command takes a look at the current directory and, if present, will
glean useful information from the `.gha` assembly and use it generate a
`manifest.yml` with name, version, authors, etc. pre-populated. If you haven't
added this information, then placeholders will be used.

Open the manifest file with your [favourite editor](https://code.visualstudio.com)
and fill in the gaps.

Afterwards, you should have something that looks a little like this...

```yaml
---
name: marmoset
version: 1.0.0
authors:
- Park Ranger
description: >
  This plug-in does something. I'm not really sure exactly what it's supposed to
  do, but it does it better than any other plug-in.
url: https://example.com
keywords:
- mammal
```

Now that we have a manifest file, we can build the package!

```commandline
> "{{ site.rhino.windows_path }}\System\Yak.exe" build

Building package from contents of C:\Users\Bozo\dist

Found manifest.yml for package: marmoset (1.0.0)
Inspecting content: Marmoset.gha
Creating marmoset-1.0.0-rh6_18-any.yak

---
name: marmoset
version: 1.0.0
authors:
- Will Pearson
description: >
  This plug-in does something. I'm not really sure exactly what it's supposed to
  do, but it does it better than any other plug-in.
url: example.com
keywords:
- mammal
- guid:c9beedb9-07ec-4974-a0a2-44670ddb17e4

C:\Users\Bozo\dist\marmoset-1.0.0-rh6_18-any.yak
├── Marmoset.dll
├── Marmoset.gha
├── manifest.yml
├── misc\LICENSE.txt
└── misc\README.md
```

<div class="alert alert-info" role="alert">
<strong>Note:</strong> The filename includes a <a href="../the-anatomy-of-a-package#distributions">"distribution tag"</a> (in this case <code>rh6_18-any</code>). The first part, <code>rh6_18</code>, is inferred from the version of Grasshopper.dll or Rhinocommon.dll that is referenced in the plug-in project. The second part, <code>any</code>, refers to the platform that the plug-in is intended for. To build a platform-specfic package, run the <code>build</code> command again with the <code>--platform &lt;platform&gt;</code> argument, where <code>&lt;platform&gt;</code> can be either <code>win</code> or <code>mac</code>.
</div>

<div class="alert alert-info" role="alert">
<strong>Note:</strong> You might notice your plug-in's GUID lurking in the
keywords. More information on how this is used can be found in the
<a href="../package-restore-in-grasshopper">"Package Restore in Grasshopper"
</a> guide.
</div>

Congratulations! 🙌 You've just created a package for your Grasshopper plug-in.

---

## Next Steps

Now that you've created a package, [push it to the package server](../pushing-a-package-to-the-server) to make it
available in the package manager!

## Related Topics

- [Package Manager Guides and Tutorials]({{ site.baseurl }}/guides/yak/)
- [Creating a Rhino Plug-in Package]({{ site.baseurl }}/guides/yak/creating-a-rhino-plugin-package/)
- [Package Restore in Grasshopper]({{ site.baseurl }}/guides/yak/package-restore-in-grasshopper/)
- [Grasshopper: Your First Component (Windows)]({{ site.baseurl }}/guides/grasshopper/your-first-component-windows/
- [Grasshopper: Your First Component (Mac)]({{ site.baseurl }}/guides/grasshopper/your-first-component-mac/
