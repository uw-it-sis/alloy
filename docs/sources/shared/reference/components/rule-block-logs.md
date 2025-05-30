---
canonical: https://grafana.com/docs/alloy/latest/shared/reference/components/rule-block-logs/
description: Shared content, rule block logs
headless: true
---

The `rule` block contains the definition of any relabeling rules that can be applied to an input log.
The transformations are applied in top-down order if more than one `rule` block is defined.

You can use the following arguments to configure a `rule`.
All arguments are optional. Omitted fields take their default values.

| Name            | Type           | Description                                                                                                                                                                                                                                   | Default | Required |
| --------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | -------- |
| `action`        | `string`       | The relabeling action to perform.                                                                                                                                                                                                             | replace | no       |
| `modulus`       | `uint`         | A positive integer used to calculate the modulus of the hashed source label values.                                                                                                                                                           |         | no       |
| `regex`         | `string`       | A valid RE2 expression with support for parenthesized capture groups. Used to match the extracted value from the combination of the `source_label` and `separator` fields or filter labels during the `labelkeep/labeldrop/labelmap` actions. | `(.*)`  | no       |
| `replacement`   | `string`       | The value against which a regular expression replace is performed if the regular expression matches the extracted value. Supports previously captured groups.                                                                                 | `"$1"`  | no       |
| `separator`     | `string`       | The separator used to concatenate the values present in `source_labels`.                                                                                                                                                                      | ;       | no       |
| `source_labels` | `list(string)` | The list of labels whose values are to be selected. Their content is concatenated using the `separator` and matched against `regex`.                                                                                                          |         | no       |
| `target_label`  | `string`       | Label to which the resulting value is written to.                                                                                                                                                                                             |         | no       |

You can use the following actions:

* `drop`      - Drops logs where `regex` matches the string extracted using the `source_labels` and `separator`.
* `dropequal` - Drop targets for which the concatenated `source_labels` do match `target_label`.
* `hashmod`   - Hashes the concatenated labels, calculates its modulo `modulus`, and writes the result to the `target_label`.
* `keep`      - Keeps logs where `regex` matches the string extracted using the `source_labels` and `separator`.
* `keepequal` - Drop targets for which the concatenated `source_labels` don't match `target_label`.
* `labeldrop` - Matches `regex` against all label names. Any labels that match are removed from the log's label set.
* `labelkeep` - Matches `regex` against all label names. Any labels that don't match are removed from the log's label set.
* `labelmap`  - Matches `regex` against all label names. Any labels that match are renamed according to the contents of the `replacement` field.
* `lowercase` - Sets `target_label` to the lowercase form of the concatenated `source_labels`.
* `replace`   - Matches `regex` to the concatenated labels. If there's a match, it replaces the content of the `target_label` using the contents of the `replacement` field.
* `uppercase` - Sets `target_label` to the uppercase form of the concatenated `source_labels`.

{{< admonition type="note" >}}
The regular expression capture groups can be referred to using either the `$CAPTURE_GROUP_NUMBER` or `${CAPTURE_GROUP_NUMBER}` notation.
{{< /admonition >}}
