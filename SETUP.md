# Setup do README — Alvaro-Sousa

Um workflow gera a snake animation (grid de commits animado) pro README.
O `gh` (GitHub CLI) já está instalado e autenticado na sua conta.

---

## 1. Enviar os arquivos

```bash
cd ~/Alvaro-Sousa
git add .
git commit -m "feat: readme animado com stats e snake"
git push -u origin main
```

> O repo `Alvaro-Sousa` **já existe** no seu GitHub.
> Se o `push` reclamar de remote:
> ```bash
> git remote add origin https://github.com/Alvaro-Sousa/Alvaro-Sousa.git
> ```

> ⚠️ Se o GitHub recusar com *"without `workflow` scope"*, o token do `gh`
> precisa de autorização extra. Rode:
> ```bash
> gh auth refresh -h github.com -s workflow
> ```
> e autorize no navegador.

---

## 2. Criar o secret

```bash
TOKEN=$(gh auth token)
gh secret set GH_PAT --repo Alvaro-Sousa <<< "$TOKEN"
gh secret list --repo Alvaro-Sousa
```

Escopo `repo` cobre o que o workflow precisa: ler seus dados e commitar o SVG.

---

## 3. Rodar o workflow

Na primeira vez, pela interface:

1. Repo → aba **Actions**
2. Selecione **Generate snake animation** → **Run workflow**

Pelo terminal:

```bash
gh workflow run snake.yml --repo Alvaro-Sousa
gh run watch --repo Alvaro-Sousa
```

O workflow faz um commit automático com o SVG. Depois disso o `cron` mantém
atualizado todo dia — sem você precisar mexer mais.

---

## 4. Conferir

```bash
gh api repos/Alvaro-Sousa/Alvaro-Sousa/contents/output/github-snake.svg && echo "snake OK"
```

Depois é só abrir https://github.com/Alvaro-Sousa e dar F5.

---

## Manutenção

| O quê | Quando |
|---|---|
| Token | `gh auth refresh` quando o `gh` pedir |
| Workflow | Automático, todo dia às 00:00 UTC |

---

## Troubleshooting

| Sintoma | Causa |
|---|---|
| README não aparece no perfil | Repo precisa ser público e ter o nome igual ao usuário (`Alvaro-Sousa`) |
| Snake não aparece | Secret não cadastrado, ou workflow ainda não rodou |
| Push rejeitado com `workflow scope` | `gh auth refresh -h github.com -s workflow` e autorize |
| Trophy não carrega | O endpoint oficial `github-profile-trophy.vercel.app` foi descontinuado. O README usa o mirror `profile-trophy.vercel.app` |
| Stats com 404 | Usuário no link não bate — é `Alvaro-Sousa` com maiúscula |

---

## Notas

**Stats e privacidade.** Os cards de stats usam **somente dados públicos**
(sem `count_private` e sem `include_all_commits`). Isso mantém o card
consistente com o que é visível no seu perfil, sem inflar números com
repositórios privados.

**Instagram.** O username correto é **`@alvru_s`** (com underscore). Instagram
não aceita hífen em username.

**metrics.svg foi removido.** A action `anuraghazra/github-profile-metrics` foi
tirada do GitHub e não tem substituto confiável. O card era redundante com os
stats que já estão no README.