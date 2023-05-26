# bas[h]ash
An exploratory project for producing rudimentary checksums in pure BASH

Fully multimodal. Usable as a standalone executable or sourced script, with or without subshells, with or without --options and with or without copying the input and output in memory.  *See: [Examples of use](https://github.com/ulfnic/bashash#examples-of-use)*
```bash
printf '%s' 'Some Text' | bashash -
# stdout: 688ce7f66dfbe463623c91aa17a1081b
```

## Installation
```bash
sudo wget -O /usr/local/bin/bashash https://raw.githubusercontent.com/ulfnic/bashash/main/bashash
sudo chmod +x /usr/local/bin/bashash
```

## Syntax
```bash
bashash
  # Special
  -b|--checksum-bytes            # Optional: (default: 16) Size of the checksum in bytes
  --                             # Optional: End processing of options
  
  # Input
  -t|--text STRING               # Optional: Use STRING as input
  -n|--input-nameref VAR_NAME    # Optional: Input from variable VAR_NAME (source-dep)
  INPUT_FILE                     # Optional: Read input from INPUT_FILE
  -                              # Optional: Read input from stdin
  
  # Output
  -N|--output-nameref VAR_NAME   # Optional: Output to variable VAR_NAME (source-dep)
  -v|--output-as-var             # Optional: Output to variable bashash__output (source-dep)

# source-dep: the script must be sourced to use this feature
```

## Examples of use

### As a standalone executable
```bash
bashash ./file.txt
# stdout: 688ce7f66dfbe463623c91aa17a1081b

bashash --text 'Some Text'
# stdout: 688ce7f66dfbe463623c91aa17a1081b

printf '%s' 'Some Text' | bashash -
# stdout: 688ce7f66dfbe463623c91aa17a1081b

printf '%s' 'Some Text' | bashash --checksum-bytes 8 -
# stdout: 688ce7f66dfbe463
```

### As a sourced script
```bash
source /usr/local/bin/bashash
```
All standalone executable examples above apply in addition to...
```bash
# Input from a specific variable
text_str='Some Text'
bashash --input-nameref 'text_str'
# stdout: 688ce7f66dfbe463623c91aa17a1081b

# Output to a specific variable
bashash --text 'Some Text' --output-nameref 'my_output'
printf '%s\n' "$my_output"
# stdout: 688ce7f66dfbe463623c91aa17a1081b

# Output to bashash__output
bashash --text 'Some Text' --output-as-var
printf '%s\n' "$bashash__output"
# stdout: 688ce7f66dfbe463623c91aa17a1081b

# Skip the option wrapper and use bashash__main directly
bashash__input='Some Text' bashash__checksum_bytes=8 bashash__main
printf '%s\n' "$bashash__output"
# stdout: 688ce7f66dfbe463
```

## License
Licensed under GNU Affero General Public License v3. See LICENSE for details.
