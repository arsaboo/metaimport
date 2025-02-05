# beets-metaimport

A [beets](https://github.com/beetbox/beets) plugin that imports metadata from multiple sources in order of preference.

## Currently Supported Sources

The plugin currently supports the following metadata sources:
- `youtube` (requires beets-youtube plugin)
- `jiosaavn` (requires beets-jiosaavn plugin)

More sources will be added in future updates.

## Installation

```bash
pip install beets-metaimport
```

## Configuration

Add `metaimport` to your beets configuration file's plugins section:

```yaml
plugins: [..., metaimport]

metaimport:
    sources:
        - jiosaavn  # Only use supported sources listed above
        - youtube
    exclude_fields:
        - id
        - path
    merge_strategy: priority  # or 'all' to collect all unique values
```

### Configuration Options

- `sources`: List of metadata sources in order of preference. Values from sources listed earlier will take precedence in case of conflicts. **Only include supported sources listed above.**
- `exclude_fields`: List of fields to exclude from metadata import
- `merge_strategy`: How to handle conflicting values:
  - `priority`: Use values from the first source that provides them (default)
  - `all`: Collect all unique values in a list

## Usage

```bash
beet metaimport [query]
```

The plugin will:
1. Search for tracks matching your query
2. For each track:
   - Search each configured source using track information
   - Try both direct track lookup and album-based lookup
   - Use fuzzy matching to find the best matching tracks
3. Merge the metadata according to your configuration
4. Apply the merged metadata to your tracks

### Examples

Import metadata for all tracks:
```bash
beet metaimport
```

Import metadata for specific artist:
```bash
beet metaimport artist:Beatles
```

Import metadata for an album:
```bash
beet metaimport album:"Abbey Road"
```

## Troubleshooting

If you're having issues:

1. Check your configuration:
   - Ensure you're only using supported sources (youtube, jiosaavn)
   - Verify required source plugins are installed
   - Make sure source plugins are properly configured

2. Enable debug logging in your beets config:
```yaml
verbose: yes
```

3. Common issues:
   - "Unsupported source plugin": Check that you're only using supported sources listed above
   - "No metadata sources available": Check your configuration and ensure required plugins are installed
   - "No metadata found": Try adjusting your query or check if the track exists in the configured sources
   - Source plugin errors: Check the specific source plugin's configuration

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. If you'd like to add support for a new metadata source, please check the existing source implementations for reference.

## License

MIT
