# ![MEN Stack Relating Data Lab - Cookbook - Setup](./assets/hero.png)

## Setup

Open your Terminal application and navigate to your `~/code/ga/labs` directory:

```bash
cd ~/code/ga/labs
```

## Cloning the Auth boilerplate

This lab uses the [`MEN Stack Auth Template`](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template) as starter code. Doing so allows us to have a connection established to our MongoDB Atlas, add functioning auth for our user model, and install some of the packages we will need for our app build.

Navigate to the [MEN Stack Auth Template](https://git.generalassemb.ly/modular-curriculum-all-courses/men-stack-session-auth-template) and clone the repository to your machine:

```bash
git clone git@git.generalassemb.ly:modular-curriculum-all-courses/men-stack-session-auth-template.git
```

Once cloned, rename this application to today's lab `men-stack-relating-data-lab-cookbook`:

```bash
mv men-stack-session-auth-template men-stack-relating-data-lab-cookbook
```

Next, `cd` into your renamed directory:

```bash
cd men-stack-relating-data-lab-cookbook
```

Finally, remove the existing `.git` information from this template:

```bash
rm -rf .git
```

> Removing the `.git` info is important as this is just a starter template provided by GA. You do not need the existing git history for this project.


### GitHub setup

To add this project to GitHub, initialize a Git repository:

```bash
git init
```

Make a new repository on [GitHub](https://github.com/) named `men-stack-relating-data-lab-cookbook`. 

Link your local project to your remote GitHub repo:

```bash
git remote add origin https://github.com/<github-username>/men-stack-relating-data-lab-cookbook.git
git push origin main
```

> 🚨 Do not copy the above command. It will not work. Your GitHub username will replace `<github-username>` (including the `<` and `>`) in the URL above.

Open the project's folder in your code editor:

```bash
code .
```

Happy Coding! 