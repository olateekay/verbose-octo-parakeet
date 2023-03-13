This is an improved dockerfile with improved security, build speed and clarity.

1.) First of, we pin the base image to at least a minor version.
By choosing a small base image, we:
- Reduce image size(speeding up build/transfer)
- Reduce attack surface area
-  prevent breaking due to an upstream update


2.) Second, let's set the working directory (👁️)

By default, the working directory would be the root path (/) but you should set it to something else based on the conventions of your specific language + framework.

This will provide a dedicated place in the filesystem with your app.

3.)Third, COPY our package.json & package-lock.json files separate from our source code (🏎️)

Docker images are cached at each layer (command). This change prevents every code change from invalidating the cache so we only need to reinstall the dependencies if we change one of them!


4.)Fourth, use a non-root USER (🔒)

If configured properly, containers provide some protection (via user namespaces) between root users inside a container, but setting to a non-root user provides another layer to our defense in depth security approach!


5.)Fifth, configure the app for production (🔒 + 🏎️)

The NODE_ENV=production environment changes how certain utilities behave, increasing performance.

Using npm ci instead of npm install ensures a reproduceable build

--only production prevents installing needed dev dependencies


6.)Sixth, use the EXPOSE instruction (👁️)

EXPOSE documents to users of the image which port the application expects to be listening on. You will still need to publish the port at runtime, but this makes it clear to end users what to expect!


7.)Seventh, use cache mounts! (🏎️)

By leveraging the cache mount feature, we can speed up the installation of dependencies by utilizing a local cache where possible.


8.)Finally, use multi-stage builds to separate dev from production! (👁️)

In order to have an ergonomic dev environment with dev dependencies, hot reloading, etc... but retain the production improvements for deploying we create separate stages in the Dockerfile!
