# production ready dockerfile that runs pnpm start
FROM node:20-alpine

ARG PORT=3000
EXPOSE ${PORT}

# set working directory
WORKDIR /app

# install typescript
RUN npm add -g typescript

# copy package.json and pnpm-lock.yaml
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./

# install pnpm and create global pnpm symlink
RUN corepack install && corepack enable

# copy source code
COPY . .

RUN pnpm fetch

# install dependencies
RUN pnpm install -r

# copy source code
RUN pnpm build

# remove dev dependencies
# RUN pnpm clean-modules

# install dependencies
# RUN pnpm install -r

# Always use JSON logs in production
ENV ALTO_JSON=true

# start app
CMD ["/bin/sh", "-c", "pnpm start ${ENTRYPOINTS:+--entrypoints $ENTRYPOINTS} ${RPC_URL:+--rpc-url $RPC_URL} ${EXECUTOR_PRIVATE_KEYS:+--executor-private-keys $EXECUTOR_PRIVATE_KEYS} ${UTILITY_PRIVATE_KEY:+--utility-private-key $UTILITY_PRIVATE_KEY} ${ENTRYPOINT_SIMULATION_CONTRACT_V7:+--entrypoint-simulation-contract-v7 $ENTRYPOINT_SIMULATION_CONTRACT_V7} --safe-mode=false --deploy-simulations-contract=false --balance-override=false --code-override-support=false"]
