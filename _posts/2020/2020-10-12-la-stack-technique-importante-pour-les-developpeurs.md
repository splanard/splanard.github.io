---
layout: post
title: "La « stack technique » : pourquoi les développeurs y attachent-ils autant d'importance ?"
date: 2020-10-12 09:00:00 +0100
tags: article tech
---

_Cet article est une version révisée d'un [article initialement publié sur LinkedIn](https://www.linkedin.com/pulse/la-stack-technique-pourquoi-les-d%C3%A9veloppeurs-y-autant-planard/)._

_J'utilise dans cet article le terme «&nbsp;développeur&nbsp;» au masculin. Je suis conscient qu'il existe des développeuses et que les femmes ont toute leur place dans l'industrie logicielle&nbsp;! Je me refuse pour autant à écrire systématiquement «&nbsp;un(e) développeur(se)&nbsp;» ou «&nbsp;un·e développeur·se&nbsp;» car je trouve cela aussi insupportable à lire qu'à écrire. Je compte donc sur mes lecteurs·trices (vous voyez, c'est lourd !_&nbsp;😣*) pour ne pas s'en formaliser. Veuillez considérer ici «&nbsp;développeur&nbsp;» comme un terme non genré, comme il l'est en anglais.*

Vous êtes-vous déjà retrouvé(e) perplexe devant l'obsession d'un développeur pour la _stack technique_ d'un projet ou d'une mission&nbsp;? Avez-vous des difficultés à recruter des développeurs pour travailler sur un projet avec une techno vieillissante&nbsp;?&nbsp;🤔

Dans cet article, je vais tenter de vous faire comprendre pourquoi les développeurs attachent autant d'importance à la composition technique des projets sur lesquels ils sont amenés à travailler.

# Des technos toujours plus nombreuses

Commençons d'abord par définir ce terme de «&nbsp;stack technique&nbsp;». C'est un anglicisme très couramment utilisé dans le domaine du développement logiciel et qui représente **l'ensemble des technologies et composants utilisés pour développer et faire tourner un produit logiciel**. Une alternative en bon français serait «&nbsp;environnement technique&nbsp;».

Par exemple, le développement et le fonctionnement d'un site web nécessitent souvent, au minimum&nbsp;:

- un moteur de base de données,
- un langage côté serveur,
- très fréquemment une librairie et/ou un framework d'affichage côté client,
- un serveur applicatif,
- une solution d'hébergement.

Et ce n'est vraiment qu'un strict minimum&&nbsp;! Les combinaisons des différentes technologies sont infinies, et il y a de nombreuses catégories que je n'ai même pas citées ici. Je ne m'essaierai pas à l'exercice de tenter de tout citer&nbsp;: d'une part c'est impossible, et d'autre part ce n'est vraiment pas l'objet de l'article.

Ci-dessous, un graphique emprunté à l'[étude Stack Overflow 2020](https://survey.stackoverflow.co/) qui représente les liens existants entre les principales technologies mentionnées par les développeurs. Là encore, la liste est très loin d'être exhaustive&nbsp;!

<p>
    <a href="/assets/images/2020/stack_overflow_links_between_techs.svg" target="_blank">
        <img src="/assets/images/2020/stack_overflow_links_between_techs.svg" />
    </a>
</p>
<p class="legend">Cliquez sur l'image pour l'ouvrir en grand</p>

**Ce paysage technologique est en perpétuelle évolution**&nbsp;: de nouveaux langages et outils apparaissent chaque année, alors que d'autres tombent peu à peu en désuétude. Et cette évolution s'accélère.

_Edit: depuis 2020, Stack Overflow a publié d'autres études, dont la consultation peut être instructive._&nbsp;😉

# Une question de goûts

Face à un aussi vaste choix de technologies, à la multiplicité des profils de développeurs et à la grande diversité des êtres humains, **des préférences personnelles apparaissent forcément**. Chaque développeur possède donc ses technologies favorites. Et, à l'inverse, il y en a d'autres avec lesquelles il n'apprécie pas de travailler.&nbsp;🤷

L'étude Stack Overflow le montre chaque année&nbsp;! En 2020, Rust, TypeScript et Python ont la cote auprès des développeurs, alors que VBA, Objective-C et Perl tiennent le top 3 des langages les plus mal-aimés. PHP, un langage pourtant extrêmement répandu dans le domaine du web, se trouve très souvent dans le top 5 des langages les moins appréciés (on le retrouve en sixième position en 2020).

![](/assets/images/2020/stack_overflow_langages_plus_apprecies.png)

<p class="legend">Langages les plus appréciés en 2020</p>

![](/assets/images/2020/stack_overflow_langages_moins_apprecies.png)

<p class="legend">Langages les moins appréciés en 2020</p>

Pourtant, ce sont tous des langages de programmation, qui impliquent tous une logique de base similaire&nbsp;: des variables, des conditions, des opérateurs, etc. Mais chaque langage ou framework possède ses spécificités, sa propre logique interne, ses grands principes de fonctionnement. Des choix de conception qui plaisent ou non aux développeurs qui vont les utiliser, et qui sont plus ou moins dans l'air du temps (Certains langages répondent également mieux que d'autres à des problématiques techniques bien spécifiques).

Un framework peut être exactement ce que la communauté des développeurs recherche pendant un temps, et devenir vieillot ou obsolète quelques années plus tard. C'est un secteur très sensible à l'_effet de mode_&nbsp;!

ci-dessous, une vidéo qui montre l'évolution de la popularité des langages de programmation depuis 1965&nbsp;:

<iframe width="560" height="315" src="https://www.youtube.com/embed/UNSoPa-XQN0?si=SOZgPyI3zptnYXv_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen style="display: block; margin: 0 auto 12px auto;; border-radius: 8px;"></iframe>

_Pour chaque langage, il faut imaginer qu'il y a un ensemble de frameworks/librairies dont la popularité varie de la même façon..._

Observez&nbsp;:

- la lente descente de Fortran, jusqu'à sa disparition du classement en 1998.
- la suprématie de C au début des années 90
- la vitesse à laquelle Java et JavaScript se propulsent dans le top 3 du classement à la fin des années 90, suivis de près par PHP au début des années 2000 (en quelques années à peine).
- la popularisation de Python après 2010

Cette apparition de nouvelles technos, suivie soit de leur disparition presque aussi rapide, soit de leur gain en popularité, est **un phénomène permanent**.

J'ai réfléchi à une façon simple de faire comprendre ces différences entre les langages et frameworks à des non-développeurs. La meilleure image que j'ai trouvée est **la musique**&nbsp;🎶. Beaucoup d'êtres humains apprécient écouter de la musique. Mais il existe un nombre incroyable de styles de musique différents, certains proches les uns des autres, certains complètement opposés&nbsp;! Vous avez probablement un style de musique favori et, au moins, un style de musique que vous n'appréciez pas. Sans trop savoir expliquer pourquoi, certaines musiques vous plaisent, d'autres non.

> Les différentes technologies informatiques sont pour les développeurs ce que les différents styles de musique sont pour les mélomanes.

Certains développeurs auront un spectre très large et apprécieront de travailler avec n'importe quelle technologie. D'autres ne jurent que par un langage ou framework bien précis&nbsp;! Et, bien évidemment, on trouve toutes les nuances entre ces deux extrêmes.

Dans tous les cas, gardez à l'esprit que, **pour un développeur, travailler avec un langage ou un framework qu'il n'apprécie pas, c'est comme si vous étiez contraint(e) d'écouter toute la journée un style musical que vous n'aimez pas...**&nbsp;😣. Difficile, n'est-ce pas&nbsp;?

# Une question de survie

Vous l'avez compris, tout évolue vite et perpétuellement dans le domaine du développement logiciel. **Pour rester à la page, un développeur doit donc se former toute sa vie**, suivre cette évolution pour ne pas rester "bloqué" sur des technologies vouées à tomber dans l'oubli.

On le sait, le marché de l'emploi dans le secteur des technologies de l'information est en pénurie de profils qualifiés. Pour autant, les entreprises qui recrutent n'en sont pas moins exigeantes&nbsp;!! Tout le monde cherche le mouton à cinq pattes, LE développeur qui a une expérience de 5 ans dans chacune des technos de la stack utilisée par l'entreprise... (parfois même des technos qui n'ont pas 5 ans d'existence d'ailleurs, ça s'est déjà vu 😅). Et **au plus une entreprise cherche un développeur expérimenté, au plus ces attentes sont importantes**.

Aujourd'hui, on attend d'un profil fullstack expérimenté qu'il maîtrise, au moins, un framework Javascript moderne (Angular, Vue, React) pour la partie _frontend_ et/ou qu'il ait une expérience sur une architecture en micro-services pour la partie _backend_. Parce que c'est dans l'air du temps... Je l'ai personnellement constaté récemment.

**L'idée qu'un bon développeur est quelqu'un qui sait apprendre**, et pas quelqu'un qui sait déjà faire, **est malheureusement loin d'être partagée par tout le monde**. Ce sont les technos mentionnées sur le CV (donc avec lesquel le développeur a déjà travaillé) qui priment&nbsp;!

Un étudiant ou une jeune diplômée n'ayant pas encore de vie de famille peut faire de la veille sur son temps libre, réaliser quelques projets perso (les fameux _side projects_) pour compléter ses compétences. Encore faut-il avoir l'énergie de s'y remettre le soir après une journée passée à développer...&nbsp;😞

Avec les années, cette motivation peut venir à manquer. Des contraintes personnelles (comme un couple ou des enfants, par exemple) peuvent s'ajouter. **Faire de la veille technologique sur son temps personnel n'est pas toujours possible**. Et rares sont les entreprises qui laissent à leurs salariés le temps libre nécessaire pour le faire sur leurs horaires de travail.

**L'expérience professionnelle devient alors la seule source de renouvellement pour les compétences d'un développeur&nbsp;!** Pour que son CV reste compétitif, un développeur va donc naturellement s'orienter vers des postes avec un environnement technique récent. C'est une façon de **conserver un profil attractif** dans un secteur où la stagnation technique peut s'avérer dangereuse pour l'employabilité.

# Un enjeu de taille pour les projets

Pour vivre, un produit logiciel a besoin de développeurs. Comme on l'a vu, ces développeurs ont eux-mêmes besoin d'une communauté et d'un environnement technique attrayant. Vous comprendrez vite l'importance du choix des technologies utilisées dans un projet informatique&nbsp;!&nbsp;🤯

**Chaque choix technologique vous prive d'une partie des développeurs du marché**. Le paysage étant extrêmement changeant, le temps vous prive également, petit à petit, d'une partie de ces développeurs. Chaque technologie de votre projet est une [dette technique]({% link _posts/2018/2018-03-14-comprendre-la-dette-technique.md %}) en puissance ! Et un projet qui se repose exclusivement sur des technologies vieillissantes et/ou non appréciées de la communauté des développeurs est voué à être privé, un jour, de toute main d'œuvre.

![](/assets/images/2020/commitstrip_le_dernier_mainteneur.jpg)

<p class="legend">Image issue de <a href="https://www.commitstrip.com/fr/?">CommitStrip</a>, le blog qui raconte en images la vie des développeurs</p>

**Le renouvellement régulier de ces technologies est donc une question de survie pour les entreprises, autant que pour les développeurs&nbsp;!**

_En complément de cette lecture, je vous recommande l'article [La dette technique tue les grenouilles](https://blog.ippon.fr/2018/03/15/la-dette-technique-tue-les-grenouilles/), qui évoque l'impact psychologique de la dette technique sur les développeurs._
