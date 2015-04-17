#Translation

The Asgard Translation package helps you export and import translations.

- [Export CSV](#export-csv)
- [Export YAML](#export-yaml)
- [import CSV](#import)

<a name="export-csv"></a>
##Export CSV
Export new translations to a CSV file, along with original translations.

Usage:

	php console translation:export-csv [srcLocale] [dstLocale] [file]

<a name="export-yaml"></a>
##Export YAML
Export new translations to a YAML file.

Usage:

	php console translation:export-yaml [srcLocale] [dstLocale] [file]

<a name="import"></a>
##Import CSV
Import new translations from a CSV file and export them to YAML.

Usage:

	php console translation:import [srcFile] [dstFile]