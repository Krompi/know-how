# Gitflow

## develop-Branch erzeugen

Dies wird nur am Anfang des Projekts gemacht

    git branch develop
    git push -u origin develop

Es gibt jetzt zwei Branches: master und develop

Im develop-Branch geschieht prinzipiell die Entwicklung.

Gibt es spezielle, gekapselte Entwicklungen können diese in einen feature/xyz-Branch ausgelagert werden.

## neuen Feature-Branche erzeugen und bearbeiten

    git checkout -b feature/feature-1 develop

Damit wird ein neuer Feature-Branch erzeugt und dieser ausgecheckt.

In diesem wird ganz normal gearbeitet und Commits erzeugt.

Wichtig ist, dass dieser Branch **regelmäßig** mit dem develop-Branch abgeglichen wird:

    git merge --no-ff development

## Das Feature ist abgeschlossen

Wenn das Feature fertig ist, wird dieses in den develop-Branch gemergt

    git checkout develop
    git merge --no-ff feature/feature-1

Die Option --no-ff bewirkt, dass nur ein merge-Commit erzeugt wird

## neuer Release-Branch

Ist alles vorbereitet für ein neues Release wird eine neuer Branch angelegt

    git checkout -b release/v1.0 develop

Hier kann getestet werden und evlt. noch kleinere Bugfixes durchgeführt werden.

Ist das Release fertig wird es final in den Master-Branch übertragen und ggf. mit einem Tag versehen:

    git checkout master
    git merge --no-ff release/v1.0
    git tag -a v1.0

Wichtig ist auch, dass der develop-Branch auch identisch mit dem Release-Branch ist.

    git checkout develop
    git merge --no-ff release/v1.0

## aufräumen

Nach erfolgreichem Release kann überlegt werden, welche Branches noch benötigt werden und welche gelöscht werden können:

    # Release-Branch löschen
    git branch -d release/v1.0
    # ggf. Feature-Branch löschen
    git branch -d feature/feature-1

## Hotfixes

Hotfixes werden direkt vom master-Branch erzeugt:

    git checkout -b hotfix/1.0.1 master

Und nach fertiger Fehlerbehebung sowohl in den master- als auch in den develop-Branch gemergt:

    git checkout master
    git merge --no-ff hotfix/1.0.1
    git checkout develop
    git merge --no-ff hotfix/1.0.1

## Graphische Darstellen

![Gitflow-Modell](pics/git-flow-model.png)

## Quellen

- https://nvie.com/posts/a-successful-git-branching-model/
- https://www.atlassian.com/de/git/tutorials/comparing-workflows/gitflow-workflow

