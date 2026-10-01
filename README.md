# Latest PHP Releases Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Latest%20PHP%20Releases-blue?logo=github)](https://github.com/marketplace/actions/latest-php-releases)

A GitHub Action to fetch a list of active PHP releases from [php.net](https://php.net).

## Outputs

### `releases`

A JSON array containing all active PHP release branches. Releases are sorted by version in ascending order. Each
release contains its version and the source archives reported by php.net.

```json
[
    {
        "version": "8.5.11",
        "sources": [
            {
                "filename": "php-8.5.11.tar.gz",
                "name": "PHP 8.5.11 (tar.gz)",
                "sha256": "338630ba9450f0b938bef8d740162c61c33bf64a1b421c6333c01df8a9fdb0ab",
                "date": "24 Sep 2026"
            }
        ]
    }
]
```

## Example usage

```yaml
name: PHP Latest Releases

on:
    pull_request:
    push:

jobs:
    latest-releases:
        runs-on: ubuntu-latest
        outputs:
            releases: ${{ steps.get-latest-releases.outputs.releases }}
        steps:
            - name: Checkout
              uses: actions/checkout@v7
            - uses: slpxxv/latest-php-releases-action@1.2.0
              id: get-latest-releases

    print-php-version:
        needs: ['latest-releases']
        runs-on: ubuntu-latest
        strategy:
            matrix:
                releases: ${{ fromJson(needs.latest-releases.outputs.releases) }}
        steps:
            - name: Checkout
              uses: actions/checkout@v7
            - run: echo "php-${{ matrix.releases.version }}"
```

## License

This project is licensed under the [MIT License](LICENSE).
