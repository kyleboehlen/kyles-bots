# Kyle's Bots

The splash page for my Discord bot projects. The only real point of these bots is to have small projects to work from start to finish rather quickly, what can I say it's cathartic (just INTJ things?). This splash page is built when my portfolio is deployed on my digital ocean app platform. The bots themselves are all served from a single droplet managing the different bot processes with pm2. All the repos for my bots are linked below.

## Bots

[Forza Bot](https://github.com/kyleboehlen/forza-bot)

[Disney Bot](https://github.com/kyleboehlen/disney-bot)

## Compile and Minify for Production

```sh
npm run build
```

## Deployment

Deploying the bots is handled by the [Bots Deploy](https://github.com/kyleboehlen/bots-deploy) repository that specifies the configuration and hooks for an AWS Elastic Beanstalk enviroment. After updating the bots you just run the Release Changes on the Code Pipeline for the production-bots

The Vue splash page is served from AWS Amplify and will automatically run it's pipeline whenever you push to `master`