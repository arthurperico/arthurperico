![Banner](./1.png)

- 👨‍🎓 Engenharia da computaçao - 3º Periodo
- 📕 Estudando Java


<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/arthurperico/arthurperico/output/github-contribution-grid-snake-dark.svg" />
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/arthurperico/arthurperico/output/github-contribution-grid-snake.svg" />
  <img alt="github contribution grid snake animation" src="https://raw.githubusercontent.com/arthurperico/arthurperico/output/github-contribution-grid-snake.svg" />
</picture>


name: Haskell CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-haskell@v1
      with:
        ghc-version: '8.10.3'
        cabal-version: '3.2'

    - name: Cache
      uses: actions/cache@v3
      env:
        cache-name: cache-cabal
      with:
        path: ~/.cabal
        key: ${{ runner.os }}-build-${{ env.cache-name }}-${{ hashFiles('**/*.cabal') }}-${{ hashFiles('**/cabal.project') }}
        restore-keys: |
          ${{ runner.os }}-build-${{ env.cache-name }}-
          ${{ runner.os }}-build-
          ${{ runner.os }}-

    - name: Install dependencies
      run: |
        cabal update
        cabal build --only-dependencies --enable-tests --enable-benchmarks
    - name: Build
      run: cabal build --enable-tests --enable-benchmarks all
    - name: Run tests
      run: cabal test all
