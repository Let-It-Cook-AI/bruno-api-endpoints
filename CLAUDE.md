# Let It Cook API requests (Bruno)

A Bruno request for every core-api endpoint, in Bruno's OpenCollection YAML format. `README.md`
explains how to use them.

## Layout

- `Let It Cook/workspace.yml`: the Bruno workspace.
- `Let It Cook/collections/core-api/`: one `.yml` file per request, in folders that match
  core-api's controllers (`Account`, `Pantry`, `Recipe`), plus `Auth/Sign In.yml`.
- `Let It Cook/collections/core-api/environments/`: `local.yml` and `production.yml`, holding
  `baseUrl`, `recipeId` and the secret variables.

## Conventions

- Quote every URL: `url: "{{baseUrl}}/core-api/..."`. Unquoted, YAML reads `{{...}}` as a
  mapping and Bruno can't load the file.
- Path parameters use variables such as `{{recipeId}}`, never placeholders such as `{id}`.
- Protected requests have an `auth` block under `http:` with `type: bearer` and
  `token: "{{idToken}}"`. The public ones (Register, Reset Password, Confirm Password Reset) have
  no `auth` block.
- `Auth/Sign In` stores the Firebase ID token in `idToken` with an after-response script.
- Generation requests send `"requestId": "{{$randomUUID}}"`, as the app does.
- Example bodies must pass core-api's validation: enum values in SCREAMING_SNAKE, lengths within
  the DTO limits.
- `seq` sets the order of requests within a folder.
- Secret variables (`firebaseApiKey`, `email`, `password`, `idToken`) are declared with
  `secret: true` and no value. Bruno keeps their values on each person's machine. Never write
  real values into these files.

## Keeping in sync

When a core-api endpoint is added, removed or changes its body, update the request here in the
same change, along with the app's DTOs. After editing, check that every file still parses:

```bash
python -c "import pathlib, yaml; [yaml.safe_load(p.read_text(encoding='utf-8')) for p in pathlib.Path('Let It Cook').rglob('*.yml')]"
```
