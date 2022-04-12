# pinephone_flutter_docker
docker-compose build
docker-compose up -d
docker-compose exec my_flutter /bin/bash

flutter create myapp
cd myapp
flutter build linux --target-platform=linux-arm64
tar -czvf build.tar.gz -C build/linux/arm64/release/ bundle
cp ../org.flatpak.MyApp.yml .
flatpak-builder --repo=repo --force-clean build-dir org.flatpak.MyApp.yml

ssh user@192.168.86.39 "sudo pacman -S rsync"
rsync -avz repo user@192.168.86.39:~/Downloads

ssh user@192.168.86.39 "flatpak install flathub org.gnome.Platform//40 org.gnome.Sdk//40 -y"
ssh user@192.168.86.39 "flatpak --user remote-add --no-gpg-verify myapp-repo /home/user/Downloads/repo"
ssh user@192.168.86.39 "flatpak --user uninstall myapp-repo org.flatpak.MyApp -y"
ssh user@192.168.86.39 "flatpak --user install myapp-repo org.flatpak.MyApp -y"
ssh user@192.168.86.39 "GDK_GL=gles flatpak run  org.flatpak.MyApp"
