name: Generar serpiente neon

on:
  schedule:
    - cron: "0 0 * * *"   # una vez al día
  workflow_dispatch:       # para lanzarla a mano
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - name: Generar SVG
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake-neon.svg?color_snake=#ffb300&color_dots=#161616,#3d2600,#7a4d00,#b36b00,#ffb300

      - name: Publicar en la rama output
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
