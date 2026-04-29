# Build and Publish

This project is a Jenkins plugin. Use the Maven wrapper in this repository so the build uses the expected Maven version.

## Build

Run tests and build the plugin:

```bash
./mvnw clean package -Dchangelist=265.v20260429_echarts_fix
```

Skip tests when you only need a local package:

```bash
./mvnw clean package -Dchangelist=265.v20260429_echarts_fix -DskipTests
```

The plugin file is created in `target/`:

```bash
ls target/*.hpi
```

## Version

The latest upstream Discord Notifier release is:

```text
264.v70060b_a_b_d300
```

Use a higher custom version for patched builds so Jenkins can upgrade from the official release:

```bash
./mvnw clean package -Dchangelist=265.v20260429_echarts_fix
```

For a development build, use a snapshot version:

```bash
./mvnw clean package -Dchangelist=265-SNAPSHOT
```

## Install Manually in Jenkins

1. Build the `.hpi` file.
2. Open Jenkins.
3. Go to `Manage Jenkins` > `Plugins` > `Advanced settings`.
4. Upload the generated `target/discord-notifier.hpi`.
5. Restart Jenkins if requested.

## Deploy on a Linux Jenkins Server

If Jenkins is installed as a `systemd` service, deploy the built `.hpi` directly into the Jenkins plugin directory:

```bash
./mvnw clean package -Dchangelist=265.v20260429_echarts_fix -DskipTests

sudo systemctl stop jenkins

sudo rm -f /var/lib/jenkins/plugins/discord-notifier.hpi \
  /var/lib/jenkins/plugins/discord-notifier.jpi

sudo cp target/discord-notifier.hpi /var/lib/jenkins/plugins/

sudo chown jenkins:jenkins /var/lib/jenkins/plugins/discord-notifier.hpi

sudo systemctl start jenkins
```

Check that Jenkins started correctly:

```bash
sudo systemctl status jenkins
sudo journalctl -u jenkins -n 100 --no-pager
```

If Jenkins still shows the old plugin version, restart Jenkins once more or clear any stale `discord-notifier` plugin files in `/var/lib/jenkins/plugins/` before copying the new `.hpi`.

## Publish to a Jenkins Update Site

Official publishing to the Jenkins plugin update center requires Jenkins project release credentials and repository release setup.

For a private/internal Jenkins installation, publish the generated `.hpi` to your internal artifact store or plugin update site, then install that version from Jenkins.

Recommended internal release command:

```bash
./mvnw clean package -Dchangelist=265.v20260429_echarts_fix
```

If deploying with Maven to a configured internal repository:

```bash
./mvnw clean deploy -Dchangelist=265.v20260429_echarts_fix
```

Make sure your Maven `settings.xml` has credentials for the target repository before running `deploy`.
