# **Notes for small things**
----------

## **Integrating AI in your project**
----------

### **Using Gemini API key**
----------

For now, `gemini-3.5-flash` is free to use and you can integrate it to your project by using its API key. In this we will get to know about how to generate it and how to test it as well

**Step 1** -> Go to [Google AI Studio](https://aistudio.google.com/projects)

**Step 2** -> Login if not, and after this just click on `New API Key`, then create a new project and finally name the Api key you want to 

**Step 3 ->** You will get an API key, just copy it and thats it, you are ready to use it  


**Lets test it whether it works or not**

You dont need to make an app in order to test it, open your terminal (for mac) and then run these commands one after the other

```console
export GEMINI_KEY="YOUR_NEW_API_KEY"
```
Replace `YOUR_NEW_API_KEY` with the API Key you have generated.

After this to cross check whether the key has been stored or not run the below command

```console
if [ -n "$GEMINI_KEY" ]; then
  echo "Gemini key is set"
else
  echo "Gemini key is NOT set"
fi
```

And fianlly make the request by running the below command, if it responds with some answer to the question you have asked, then you are good to go

```console
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.5-flash:generateContent" \
  -H "x-goog-api-key: $GEMINI_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain Quantum particles in one or two line"
          }
        ]
      }
    ]
  }'
```

### **Using Openrouter API Key**
----------

`Openrouter` also gives many free models and you can get the response using those models as well using its API key

**Step 1** -> Go to [Openrouter](https://openrouter.ai/)

**Step 2** -> Click on `Get Api Key` button, you will be asked to log in, just log in and then generate the API Key.

**Step 3** -> After generting the api key, now run the below commands inside the terminal (of your mac) one after the other 

```console
export OPENROUTER_KEY="PASTE_YOUR_OPENROUTER_KEY_HERE"
```

After pasting the openrouter api key run the below command and if the output comes, then you are good to go

```console
curl "https://openrouter.ai/api/v1/chat/completions" \
-X POST \
-H "Authorization: Bearer ${OPENROUTER_KEY}" \
-H "Content-Type: application/json" \
-d '{
  "model": "openrouter/free",
  "messages": [
    {"role": "user", "content": "Say hello in exactly three words."}
  ]
}'
```

## **No need for Nodemon for continuous listening**
----------

Node.js version above 22 supports live hot reloading so you dont need to restart the server everytime you make changes or install nodemon for this.

Just add the `dev` script inside the `package.json` file

```javascript
"scripts": {
  "start": "node src/index.js",
  "dev": "node --watch src/index.js",  // Add this
  "test": "node --test"
}
```
then just run `npm run dev` and now no need for the server to again reload the server, it will automatically reload the server after any changes made

## **Pushing to another github account while signed in with  the main account**
----------
**Step 1 ->** Configure the commit author for this folder

Use the email connected to the `SR-Test-coder` account:

```bash
git config user.name "SECONDARY_ACCOUNT_USERNAM"
git config user.email "SECONDARY_ACCOUNT_EMAIL"
```

Confirm by running the below commands 

```bash
git config user.name
git config user.email
```

> These settings apply only to the current repository/folder in inside which you are running this command, because `--global` is not used.

## 5. Add and commit the current files

```bash
git add .
git status
```

Carefully inspect the files under “Changes to be committed.” If everything is correct:

```bash
git commit -m "Add short notes"
```

Rename the current branch to `main`:

```bash
git branch -M main
```

## 6. Log into the secondary GitHub account

First inspect the saved accounts:

```bash
gh auth status
```

Now start login:

```bash
gh auth login --hostname github.com --git-protocol https --web
```

When GitHub opens in the browser:

1. Switch to `SR-Test-coder`.
2. Confirm the displayed account is `SR-Test-coder`.
3. Authorize GitHub CLI.

Select the secondary account:

```bash
gh auth switch --hostname github.com --user SR-Test-coder
```

Configure terminal Git to use GitHub CLI authentication:

```bash
gh auth setup-git --hostname github.com
```

Verify:

```bash
gh auth status --active --hostname github.com
```

It should show:

```text
Active account: true
Account: SR-Test-coder
```

## 7. Add the secondary repository without removing an existing remote

Check existing remotes:

```bash
git remote -v
```

Add the secondary repository using a separate remote name:

```bash
git remote add sr-test https://github.com/SR-Test-coder/Short-Notes.git
```

Verify it:

```bash
git remote -v
```

You should see something similar to:

```text
sr-test  https://github.com/SR-Test-coder/Short-Notes.git (fetch)
sr-test  https://github.com/SR-Test-coder/Short-Notes.git (push)
```

If `git remote add` says:

```text
error: remote sr-test already exists
```

update the existing URL:

```bash
git remote set-url sr-test https://github.com/SR-Test-coder/Short-Notes.git
```

## 8. Push to `SR-Test-coder`

```bash
git push -u sr-test main
```

After it finishes, refresh this repository in your browser:

```text
https://github.com/SR-Test-coder/Short-Notes
```

## If GitHub rejects the push

If you created the GitHub repository with a README, you may receive a non-fast-forward error. Use:

```bash
git pull sr-test main --no-rebase --allow-unrelated-histories
```

If Git reports conflicts, resolve them in VS Code and then run:

```bash
git add .
git commit -m "Merge GitHub repository"
git push -u sr-test main
```

Do not use `--force`.

## 9. Switch terminal authentication back to your primary account

After the push succeeds:

```bash
gh auth switch --hostname github.com --user SatyamRaj1905
```

Verify:

```bash
gh auth status --active --hostname github.com
```

This does not remove the secondary account. It merely makes `SatyamRaj1905` active again.

For future updates to this particular repository:

```bash
gh auth switch --hostname github.com --user SR-Test-coder
git push sr-test main
gh auth switch --hostname github.com --user SatyamRaj1905
```

The `sr-test` remote belongs only to this repository. It will not change the remotes or destinations of your other project folders.