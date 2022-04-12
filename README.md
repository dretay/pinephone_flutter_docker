# Pinephone Flutter Docker

> Build Flutter Applications for the PinePhone with Docker

An example project that contains everything needed to build a test application for a pinephone.  <img src="https://raw.githubusercontent.com/dretay/pinephone_flutter_docker/master/screenshot.jpg" align="right" width="208">

### Build Instructions ###
#### PC Setup
1. Build the project: `docker-compose build`
2. Spin up the environment: `docker-compose up -d`
3. Open a shell in the flutter development container: `docker-compose exec my_flutter /bin/bash`

#### Application development
1. From the container shell, create a demo app: `flutter create myapp`
2. Enter the newly created directory: `cd myapp`
3. Cross compile the app for arm64: `flutter build linux --target-platform=linux-arm64`
4. Create a tarball of the released code: `tar -czvf build.tar.gz -C build/linux/arm64/release/ bundle`
5. Copy in the example flatpak definition: `cp ../org.flatpak.MyApp.yml .`
6. Build a release: `flatpak-builder --repo=repo --force-clean build-dir org.flatpak.MyApp.yml`

#### Application installation
1. Copy the released flatpak onto the phone: `rsync -avz repo user@192.168.86.39:~/Downloads`
2. Install the necessary dependencies: `ssh user@192.168.86.39 "flatpak install flathub org.gnome.Platform//40 org.gnome.Sdk//40 -y"`
3. If needed, add the repo: `ssh user@192.168.86.39 "flatpak --user remote-add --no-gpg-verify myapp-repo /home/user/Downloads/repo"`
4. If needed, uninstall the old app `ssh user@192.168.86.39 "flatpak --user uninstall myapp-repo org.flatpak.MyApp -y"`
5. Install the app: `ssh user@192.168.86.39 "flatpak --user install myapp-repo org.flatpak.MyApp -y"`
6. Run the app: `ssh user@192.168.86.39 "GDK_GL=gles flatpak run  org.flatpak.MyApp"`


## References
Based on https://github.com/nathanielgreen/flutter-pinephone-example
