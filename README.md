# FSF License Metadata API

The FSF is [interested in having the SPDX expose some of its metadata in the SPDX license list][1].
The cleanest way to do that is to have the FSF provide [their annotated license list][2] in a format that is more convenient for automated tools.
For example, [the OSI provides an API][3] which, while [currently][4] [non-canonical][5], provides convenient access to OSI license annotations.

This repository scrapes the FSF list and provides the scraped data in a JSON API for others to consume.
Ideally we'll hand this repository over to the FSF once they're ready to maintain it, or we'll deprecate this repository if they decide to provide a different API.

## Endpoints

You can pull the set of identifiers from [https://wking.github.io/fsf-api/licenses.json](https://wking.github.io/fsf-api/licenses.json).

You can pull an individual license from `https://wking.github.io/fsf-api/{id}.json`, for example [https://wking.github.io/fsf-api/Expat.json](https://wking.github.io/fsf-api/Expat.json).

## Caveats

There are currently two hacks in [the pulling script](pull.py):

* `SPLITS`, which:

    * Unpacks some places where [the FSF's HTML page][2] uses a single identifier for multiple licenses (e.g. [using `AcademicFreeLicense` for “all versions through 3.0”][6]).
    * Repacks places where [the FSF's HTML page][2] uses two identifiers for the same license (e.g. to classify `FreeBSD` as both [GPL-compatible][7] and [FDL-compatible][8]).

* `IDENTIFIERS`, which maps FSF identifiers to other schemes.
    Ideally this would be based on [automated license-text comparison][9], but in order for that to work this API would have to expose the license text that the FSF considered for each ID.
    Currently, [the FSF's HTML page][2] links to license source, but not in a consistent enough way for me to extract the text.

Until these hacks are addressed, license IDs and the `identifiers` field should be taken with a grain of salt.

## Contributing

[Contributions](CONTRIBUTING.md) are welcome!

[1]: https://lists.spdx.org/pipermail/spdx-legal/2017-October/002281.html
[2]: https://www.gnu.org/licenses/license-list.html
[3]: https://api.opensource.org/
[4]: https://github.com/OpenSourceOrg/licenses/tree/f7ff223f9694ca0d5114fc82e43c74b5c5087891#is-this-authoritative
[5]: https://github.com/OpenSourceOrg/licenses/issues/47
[6]: https://www.gnu.org/licenses/license-list.html#AcademicFreeLicense
[7]: https://www.gnu.org/licenses/license-list.html#FreeBSD
[8]: https://www.gnu.org/licenses/license-list.html#FreeBSDDL
[9]: https://github.com/spdx/license-list-XML/issues/418