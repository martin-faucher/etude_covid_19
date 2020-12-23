<!DOCTYPE html>

<html>

<head>

<meta charset="utf-8" />
<meta name="generator" content="pandoc" />
<meta http-equiv="X-UA-Compatible" content="IE=EDGE" />


<meta name="author" content="P. Cuchot, P. Fournier, M. Faucher - M2 MODE" />





</head>

<body>


<div class="container-fluid main-container">




<div class="fluid-row" id="header">



<h1 class="title toc-ignore">Le rebond de la pandémie de la Covid-19 était-il prévisible?</h1>
<h4 class="author">P. Cuchot, P. Fournier, M. Faucher - M2 MODE</h4>
<h4 class="date">23/12/2020</h4>

</div>


   $(document).ready(function() {
     $head = $('#header');
     $head.prepend('<img src=\"https://i.postimg.cc/rwfD1rXL/50627138-1958657937580223-5503718831379447808-n.png\" style=\"float: right;width: 200px;\"/>')
   });
<div id="introduction" class="section level1">
<h1>Introduction</h1>
<div align="justify">
<p>Lors de l’annonce du premier confinement général en France le 17 mars 2020, les connaissances scientifiques ainsi que les relevés d’hospitalisations permettaient déjà la publication d’articles estimant le taux d’incidence de la Covid-19 (Roques <em>et al.</em>, 2020), l’évolution de la pandémie selon les stratégies de déconfinement (Di Domenicali <em>et al.</em>, 2020) ou encore des estimations du nombre de décès évités grâce au confinement (Roux <em>et al.</em>, 2020). A l’occasion du deuxième confinement débuté le 30 octobre et ayant pris fin le 16 décembre, il est intéressant de chercher à savoir si la situation actuelle pouvait être envisagée dès la levée du premier confinement. Nous avons cherché à savoir si un modèle épidémiologique réalisé à partir des données d’hospitalisation recueillies pendant la période du confinement permettait de prévoir l’accélération du nombre de cas débutée en juillet.</p>
<p>Pour répondre à cette question, un modèle de type SIHR (figure 1) a été ajusté aux données d’hospitalisation liées à la Covid-19 pendant la période de confinement, mises à disposition sur le site de Santé Publique France. Les résultats obtenus avec ce modèle ont ensuite été comparés avec les données réelles, de la sortie du confinement à la date du 03 novembre 2020.</p>
<p>Le traitement des données ainsi que le système d’équations du modèle ont été librement inspirés de <a href="http://f.m.hamelin.free.fr/covid-19.html">F. Hamelin</a>.</p>
</div>
<div id="méthodes" class="section level1">
<h1>Méthodes</h1>
<div align="justify">
<p>Pour réaliser ce modèle, nous nous sommes intéressés aux données délivrées par les hôpitaux de France car elles sont un indicateur fiable et accessible de l’évolution de la pandémie. De plus l’effort de recensement de ces données ne devrait pas avoir beaucoup varié tout au long de la période d’étude en opposition à l’effort de test des personnes non hospitalisées.</p>
<p>Les connaissances actuelles concernant la durée d’immunité, l’existence d’une différence de contagiosité entre les formes asymptomatiques et les formes symptomatiques, ou encore les différences de réponse selon les individus nous ont poussé à garder un modèle qui soit le plus simple possible. Nous avons donc opté pour un modèle de type SIR auquel nous avons ajouté un compartiment H pour les individus hospitalisés. Nous avons aussi cherché à limiter le nombre de paramètres à estimer pour éviter le surparamétrage du modèle, mais aussi pour ne conserver que des paramètres ayant déjà été estimés dans la littérature.</p>
<div id="le-modèle-épidémiologique" class="section level2">
<h2>Le modèle épidémiologique</h2>
<div align="justify">
<p>Les hypothèses sous-jacentes au modèle utilisé sont les suivantes :</p>
<ul>
<li>La population française est spatialement homogène au sein de l’Hexagone<br />
</li>
<li>La population est divisée en groupes d’individus homogènes : tous les individus quelque soit leur âge, leur sexe et leur antécédents médicaux sont égaux vis à vis du virus<br />
</li>
<li>Le taux de mortalité lié à la maladie ne varie pas<br />
</li>
<li>La taille de la population est stable au cours du temps (pas de dynamique démographique)<br />
</li>
<li>La transmission du virus autre que par contact est négligée<br />
</li>
<li>Le R(t) est stable dans le temps<br />
</li>
<li>La guérison de la Covid-19 confère une immunité permanente<br />
</li>
<li>Les individus hospitalisés ne peuvent plus contaminer les individus susceptibles<br />
</li>
<li>Les paramètres associés au temps d’infection, d’hospitalisation et de guérison, ainsi que les taux de contagion et d’hospitalisation sont constants</li>
</ul>
<p>Les différents compartiments du modèles ont été définis de la façon suivante :</p>
<p><em><span class="math inline">\(S(t)\)</span> : Nombre d’individus sensibles au virus au temps t<br />
</em><span class="math inline">\(I(t)\)</span> : Nombre d’individus infectés et infectieux non hospitalisés au temps t<br />
<em><span class="math inline">\(H(t)\)</span> : Nombre d’individus infectés et hospitalisés au temps t<br />
</em><span class="math inline">\(R(t)\)</span> : Nombre d’individus retirés de la dynamique de l’épidémie (décédés ou immunisés) au temps t</p>
<p>Nous définissons la taille de la population tel que N = S + I + H + R = constante</p>
<p>Les paramètres du modèle sont les suivants :</p>
<p>γ : taux de sortie de l’hôpital via guérison ou décès (inverse du temps moyen passé à l’hôpital)<br />
α : taux d’hospitalisation (inverse du temps moyen avant hospitalisation)<br />
ρ : taux de sortie de l’état infecté via guérison ou décès (inverse du temps moyen avant guérison ou décès)<br />
β : taux de transmission du virus par unité de temps (R(t) * ρ).</p>
<p>Le modèle SIHR utilisé est schématisé ci-dessous …</p>
<center>
<img src="https://i.postimg.cc/xdGKVdBx/131960231-758369078133615-4874724557821101249-n.png">
</center>
<div id="figure-1-schéma-représentatif-du-modèle-sihr" class="section level4">
<h4>figure 1 : Schéma représentatif du modèle SIHR</h4>
<p>… et est traduit sous forme mathématiques ci-après:</p>
<div align="center">
<p><font size="3"><span class="math inline">\(\frac{dS}{dt}=-\beta\frac{S}{N}I\)</span></p>
<p><span class="math inline">\(\frac{dI}{dt}=\beta\frac{S}{N}I-(\alpha+\rho)I\)</span></p>
<p><span class="math inline">\(\frac{dH}{dt}=\alpha I-\gamma H\)</span></p>
<span class="math inline">\(\frac{dR}{dt}=\rho I+\gamma H\)</span> </font>
</div>
<div align="justify">
<p>Nous avons ensuite essayé d’ajuster le modèle aux données réelles pour la période du premier confinement (17 mars au 11 mai 2020). Les données en question sont le nombre de personnes hospitalisés et le nombre d’admission à l’hôpital à la date t.</p>
</div>
</div>
<div id="application" class="section level2">
<h2>Application</h2>
<div id="chargement-des-packages" class="section level3">
<h3>Chargement des packages</h3>
<pre class="r"><code>library(rmarkdown)
library(deSolve)
library(ggplot2)</code></pre>
</div>
<div id="chargement-des-données" class="section level3">
<h3>Chargement des données</h3>
<div align="justify">
<p>Les données ont été téléchargées <a href="https://www.data.gouv.fr/fr/datasets/donnees-hospitalieres-relatives-a-lepidemie-de-covid-19/">ici</a>. Les jeux de données contiennent des données hospitalières relatives à l’épidémie du COVID-19 par département et sexe du patient.</p>
<p>Etant donné que notre travail a été basé sur les données couvrant la période du début du premier confinement au 03 novembre 2020, nous avons stocké en ligne les données utilisées pour réaliser ce travail.</p>
<pre class="r"><code>dataH &lt;- read.csv(&quot;https://raw.githubusercontent.com/martin-faucher/etude_covid_19/main/donnees-hospitalieres-covid19-2020-11-03-19h00.csv&quot;, sep = &quot;;&quot;)
dataA &lt;- read.csv(&quot;https://raw.githubusercontent.com/martin-faucher/etude_covid_19/main/donnees-hospitalieres-nouveaux-covid19-2020-11-03-19h00.csv&quot;, sep = &quot;;&quot;)</code></pre>
<p>dataH contient : nombre de patients hospitalisés, nombre de personnes actuellement en réanimation ou soins intensifs, nombre cumulé de personnes retournées à domicile, nombre cumulé de personnes décédées.</p>
<p>dataA contient des données quotidiennes : nombre quotidien de personnes nouvellement hospitalisées, nombre quotidien de nouvelles admissions en réanimation, nombre quotidien de personnes nouvellement décédées, nombre quotidien de nouveaux retours à domicile.</p>
</div>
<div id="sélection-des-données-dinterêt-pour-le-projet" class="section level3">
<h3>Sélection des données d’interêt pour le projet</h3>
<div align="justify">
<p>Nous avons d’abord sélectionné les données d’interêt pour répondre à la question posée, soit les données relatives aux hospitalisations et admissions par département, agrégeant hommes et femmes.</p>
<p>Dans une volonté de clarté du document, les lignes de codes permettant de préparer le jeu de données à l’analyse, ne sont pas présentées dans le document. Vous pouvez retrouver le traitement des données directement dans le script ayant servi de base à ce document.</p>
<div id="visualisation-des-données-à-partir-du-19-mars" class="section level4">
<h4>Visualisation des données à partir du 19 mars</h4>
<pre class="r"><code>plot(1:LH,H,xlab=&quot;Temps écoulé depuis le 19 mars (en jours)&quot;,
     ylab=&quot;Nombre de personnes hospitalisées&quot;, 
     main = &quot;Figure 2a : Hospitalisations (19 mars - 11 mai 2020)&quot;, col=&quot;blue&quot;, xlim=c(0,50))</code></pre>
<p><img src="rapport_MEPI_Cuchot_Faucher_Fournier_files/figure-html/unnamed-chunk-6-1.png" width="672" /></p>
<pre class="r"><code>plot(1:LA,A,xlab=&quot;Temps écoulé depuis le 19 mars (en jours)&quot;,
     ylab=&quot;Nombre d&#39;admissions à l&#39;hôpital (par jour)&quot;,
     main = &quot;Figure 2b : Admissions à l&#39;hôpital (19 mars - 11 mai 2020)&quot;, col=&quot;red&quot;,xlim=c(0,50))</code></pre>
<p><img src="rapport_MEPI_Cuchot_Faucher_Fournier_files/figure-html/unnamed-chunk-6-2.png" width="672" /></p>
<div align="justify">
Nous observons une tendance croissante du nombre de personnes hospitalisées (figure 2a) ainsi que du nombre d’admission (figure 2b) sur les 15 premiers jours. Cependant, les pentes de H et A s’atténuent peu à peu. Nous attribuons ces tendances à une dynamique de transition suite à la mise en place du confinement. Le modèle utilisé ici, suppose que les paramètres sont constants au cours du temps. Pour cette raison, la simulation du modèle commencera 15 jours après le début du confinement, soit à partir du 1er Avril 2020.<br />

<div align="justify">
En outre, les données d’admission sont plus éparses que les données d’hospitalisation et semblent ainsi moins estimables. Ce phénomène peut être du à une remontée partielle des informations en fin de semaine en raison de la fermeture de certains services administratifs. Ceci se caractérise par de plus faibles admissions les week-ends et jours fériés ainsi que des pics d’admissions en début de semaine.<br />

<div align="justify">
<p>Pour ces raisons, nous appliquerons notre modèle sur les données d’hospitalisation.</p>
</div>
<div id="visualisation-des-données-tronquées-à-partir-du-1er-avril" class="section level4">
<h4>Visualisation des données tronquées à partir du 1er avril</h4>
<pre class="r"><code>plot(1:LH,H,xlab=&quot;Temps écoulé depuis le 1er avril (en jours)&quot;,
     ylab = &quot;Nombre d&#39;hospitalisations et admissions&quot;,
     main=&quot;Figure 3 : Hospitalisations et admissions journalières \n (1 avril - 11 mai 2020)&quot;,
     col=&quot;blue&quot;, ylim = c(1,40000), type=&#39;l&#39;)
points(1:LA,A, type=&#39;l&#39;)

par(bty=&quot;l&quot;)
legend(x=22, y=40000,legend=c(&quot;Personnes hospitalisées&quot;, &quot;Personnes admises&quot;), text.font=2, lty=1,
       col=c(&quot;black&quot;,&quot;blue&quot;), box.lty = 0, bty=&quot;l&quot;)</code></pre>
<p><img src="rapport_MEPI_Cuchot_Faucher_Fournier_files/figure-html/unnamed-chunk-8-1.png" width="672" /></p>
</div>
</div>
<div id="ecriture-du-modèle" class="section level3">
<h3>Ecriture du modèle</h3>
<div align="justify">
<p>Avant d’ajuster le modèle aux données françaises, il est nécessaire de leur attribuer une valeur initiale. Il est nécessaire de fixer au moins un paramètre, pour que le modèle ne soit pas sur-paramétré par rapport aux données utilisées (Sallet et Rapaport <em>in</em> <a href="http://f.m.hamelin.free.fr/covid-19.html" class="uri">http://f.m.hamelin.free.fr/covid-19.html</a>). Nous fixons donc <span class="math inline">\(\rho\)</span>, qui a été estimé en Chine par You <em>et al</em>. (2020) à 10,91 jours en moyenne.</p>
<pre class="r"><code>N &lt;- 64e6                    # Population hexagonale approximative
rho &lt;- 1/10.91                # Taux de sortie de l&#39;état infecté 
gamma &lt;- 1/14                 # Taux de sortie de l&#39;hôpital
R_0 &lt;- 1                      # Nombre de reproduction de base en confinement
beta &lt;- R_0*rho               # Taux de transmission (calcul très approximatif)
p &lt;- 0.1                      # Probabilité d&#39;être hospitalisé suite à l&#39;infection
alpha &lt;- p*rho/(1-p)          # Taux d&#39;hospitalisation
P0 &lt;- c(beta,alpha,gamma)     # Vecteur des paramètres</code></pre>
<p>Définition des variables du modèle et des conditions initiales (1er avril) :</p>
<pre class="r"><code>H0 &lt;- H[1]        # Nombre de personnes hospitalisées au 1er avril
I0 &lt;- A[1]/alpha  # D&#39;après le modèle, A(t) = alpha*I(t)
R0 &lt;- N*0.01      # Fixé arbitrairement à 1% d&#39;immunisés 
S0 &lt;- N-I0-H0-R0  # Taille de la population sensible au 1er avril
X0 &lt;- c(S0,I0,H0) # Vecteur d&#39;état.

# Construction d&#39;un vecteur de dates pour comparer le modèle et les données :
t &lt;- 0:(LH-1)</code></pre>
<p>Définition du système d’équation SIHR<br />
La fonction SIHR prend en argument 3 vecteurs : le temps, les variables d’état, et les paramètres :</p>
<pre class="r"><code>SIHR = function(t, X, P){
  beta &lt;- P[1];            # Taux de transmission
  alpha &lt;- P[2];           # Taux d&#39;hospitalisation
  gamma &lt;- P[3];           # Taux de sortie d&#39;hôpital
  
  S &lt;- X[1]
  I &lt;- X[2]
  H &lt;- X[3] # Le vecteur d&#39;état X contient S, I, et H
  
  y = beta*S*I/N;       # Nombre de nouvelles infections par jour
  
  # D&#39;après le modèle SIHR défini plus haut :
  dS = -y;              #  dS/dt = - beta*S*I
  dI = y-(alpha+rho)*I; #  dI/dt = beta*S*I - (alpha+rho)*I
  dH = alpha*I -gamma*H;#  dH/dt = alpha*I - gamma*H
  
  dX=c(dS,dI,dH);       # Renvoie dX/dt, mise en forme demandée par la fonction ode().
  
  return(list(dX));
}</code></pre>
<div align="justify">
<p>Afin d’ajuster le modèle SIHR aux données françaises, nous cherchons à estimer les paramètres alpha, beta et gamma ainsi que l’état initial du système tels qu’ils soient les plus vraisemblables d’après les données. Pour calculer cette vraisemblance, nous modélisons le processus d’observation de façon stochastique.</p>
</div>
<div id="maximisation-du-log-de-la-vraisemblance-du-modèle" class="section level3">
<h3>Maximisation du log de la vraisemblance du modèle</h3>
<div align="justify">
<p>Création d’une fonction qui calcule la log-vraisemblance d’un jeu de paramètres et de conditions initiales. La fonction logLike prend pour argument un vecteur de paramètres et de conditions initiales à estimer : beta, alpha, gamma, S, I, H.</p>
<pre class="r"><code>logLike=function(theta){
  P &lt;- theta[1:3];           # Paramètres beta (taux de transmission), alpha (taux d&#39;hospitalisation), et gamma (taux de sortie d&#39;hôpital)
  X0 &lt;- theta[4:6];          # Conditions initiales 
  
  X &lt;- ode(X0,t,SIHR,P)      # Résolution du système 
  
  h &lt;- X[,4];                # Hospitalisations théoriques : H(t)
  a &lt;- P[2]*tail(X[,3],-1);  # Admissions théoriques : alpha*I(t)
  
  LLH &lt;- dpois(H,h,log=T)    # Probabilité d&#39;observer H (loi de Poisson)
  LLA &lt;- dpois(A,a,log=T)    # Probabilité d&#39;observer A (loi de Poisson)
  LL &lt;- sum(c(LLH,LLA))      # Log transformation du produit des probabilités en somme
  return(LL);                # Renvoie la log-vraisemblance 
}</code></pre>
<p>Définition du vecteur des paramètres et des conditions initiales utilisé dans la fonction logLike.<br />
Ici, P0 = c (beta,alpha,gamma) et X0 = c(S, I, H).</p>
<pre class="r"><code>theta0 &lt;- c(P0,X0)</code></pre>
<p>La fonction optim estime les valeurs des paramètres et conditions initiales maximisant la vraisemblance du modèle.</p>
</div>
</div>
</div>
<div id="résultats" class="section level1">
<h1>Résultats</h1>
<p>Valeurs des paramètres et conditions initiales dont la vraisemblance est maximale :</p>
<pre class="r"><code>beta &lt;- opt$par[1]   # beta (taux de transmission)
alpha &lt;- opt$par[2]  # alpha (taux d&#39;hospitalisation)
gamma &lt;- opt$par[3]  # gamma (taux de sortie d&#39;hopital)</code></pre>
<p>Les valeurs des paramètres optimaux correspondent à :</p>
<p><strong>0.0641775</strong> pour le taux de transmission<br />
<strong>0.0078065</strong> pour le taux de d’hospitalisation<br />
<strong>0.0584921</strong> pour le taux de sortie de l’hopital</p>
<p>Selon le modèle, les conditions initiales optimales sont :</p>
<pre class="r"><code>S0 &lt;- opt$par[4]
I0 &lt;- opt$par[5]
H0 &lt;- opt$par[6]</code></pre>
<p>Les valeurs des conditions initiales maximisant la vraisemblance correspondent à :<br />
<strong>6.293554310^{7}</strong> individus sensibles<br />
<strong>4.037580610^{5}</strong> individus infectieux<br />
<strong>2.069943510^{4}</strong> individus infectés hospitalisés</p>
<p>Mise à jour des vecteurs des paramètres et conditions initiales avec les nouvelles valeurs estimées :</p>
<pre class="r"><code>P0 &lt;- c(beta,alpha,gamma);         # Vecteur des paramètres 
X0 &lt;- c(S0,I0,H0);                 # Vecteur des conditions initiales</code></pre>
<div id="simulation-du-modèle-avec-les-nouveaux-paramètres-et-conditions-initiales" class="section level3">
<h3>Simulation du modèle avec les nouveaux paramètres et conditions initiales</h3>
<pre class="r"><code>T=219 
t=0:T      # Mise à jour du vecteur temps</code></pre>
<p>Simulation du modèle à partir du 12 mai 2020</p>
</div>
<div id="comparaison-visuelle-de-la-simulation-et-des-données-françaises" class="section level3">
<h3>Comparaison visuelle de la simulation et des données françaises</h3>
<p><img src="rapport_MEPI_Cuchot_Faucher_Fournier_files/figure-html/unnamed-chunk-20-1.png" width="672" /></p>
<div align="justify">
<p>Depuis la levée du confinement, le nombre d’hospitalisation/jour a connu un rebond (figure 4). Le modèle ajusté sur les données françaises prédisait un maintien de la diminution du taux d’hospitalisations journalière après la levée du confinement .</p>
<div align="justify">
<p>Le modèle utilisé a été ajusté aux données réelles pendant le confinement. Or la majeure différence entre une population confinée et une population non confinée est le taux de contact.</p>
<p>Pour aller plus loin, nous essayons d’intégrer dans un second modèle l’augmentation du taux de contact entre humains à la sortie du confinement. Pour cela, nous augmentons la valeur du taux de transmission du virus (beta). Nous cherchons à visualiser pour quelle valeur de beta, le modèle ajusté aux données aurait pu prédire un rebond.</p>
</div>
<div id="utilisation-du-même-modèle-en-sortie-de-confinement-induisant-une-hausse-du-taux-de-transmission" class="section level3">
<h3>Utilisation du même modèle en sortie de confinement induisant une hausse du taux de transmission</h3>
<div align="justify">
<p>Nous simulons maintenant notre modèle ajusté en prenant pour conditions initiales, les valeurs de S, I et H estimées pour le 11 mai lors de la première simulation. Dans ce modèle nous gardons les valeurs des paramètres alpha et gamma ajustées par la fonction logLike. Nous simulons le modèle jusqu’au 03 novembre en utilisant différentes valeurs pour beta qui soient plus élevées que celle ajustée pour le premier modèle.</p>
<p>Création d’une liste contenant les simulations pour chaque valeur de beta</p>
<pre class="r"><code>X_list &lt;- list()</code></pre>
<p>Les conditions initiales correspondent aux valeurs de S, I et H estimées pour le 11 mai lors de la première simulation</p>
<pre class="r"><code>S0 &lt;- X[43,2]
I0 &lt;- X[43,3]
H0 &lt;- X[43,4]

X0 &lt;- c(S0,I0,H0)</code></pre>
<p>On récupère les paramètres optimaux, en modifiant le taux de transmission. Les valeurs de <span class="math inline">\(\beta\)</span> testées varient de 0,064 à 0,124 avec un pas de 0,01.</p>
<pre class="r"><code>i &lt;- 1 # Compteur nécessaire pour remplir X_list
for(beta in seq(0.064,0.15,by=0.01)){
  
  alpha &lt;- opt$par[2]         # alpha (taux d&#39;hospitalisation)
  gamma &lt;- opt$par[3]         # gamma (taux de sortie de l&#39;hopital)
  
  P0 &lt;- c(beta,alpha,gamma);         # Vecteur des paramètres mis à jour
  
  # Les solution du modèle sont calculées pour les paramètres et conditions initiales estimés :
  
  T=219
  t=0:T            # Mise à jour du vecteur temps
  
  X_list[[i]]=ode(X0,t,SIHR,P0)    # Calcul de la solution 
  i &lt;- i+1
}</code></pre>
<p>Affichage du nombre d’individus hospitalisés (données et modèle)</p>
<p><img src="rapport_MEPI_Cuchot_Faucher_Fournier_files/figure-html/unnamed-chunk-24-1.png" width="672" /></p>
<p><em>Nota bene</em> : les valeurs de <span class="math inline">\(\beta\)</span> pour la figure 5 sont les suivantes (couleur courbe, valeur) : rouge, 0,064 ; vert, 0,074 ; bleu, 0,084 ; cyan, 0,094 ; violet, 0,104 ; jaune, 0,114 ; gris, 0,124.</p>
</div>
</div>
<div id="discussion" class="section level1">
<h1>Discussion</h1>
<div align="justify">
<p>Le modèle ajusté sur les données françaises au cours du premier confinement a prédit un maintien de la diminution du nombre d’hospitalisations après la levée du confinement. Ce modèle ne permettait donc pas de prédire le rebond observé dans le nombre d’hospitalisations.</p>
<p>L’efficacité du confinement tient dans le fait que le taux de contact entre humains est drastiquement réduit. Ce taux de contact apparaît dans le modèle à travers le paramètre <span class="math inline">\(\beta\)</span> (taux de transmission du virus). Le modèle a été ajusté sur les données du confinement et suppose une constance de la valeur de ses paramètres au cours du temps. Il ne pouvait donc pas prévoir un rebond de l’épidémie après la levée du confinement.</p>
<p>Ici nous avons cherché pour quelles nouvelles valeurs de <span class="math inline">\(\beta\)</span> le modèle aurait pu prévoir un rebond lors du déconfinement. Pour cela nous avons fait varier les valeurs de <span class="math inline">\(\beta\)</span> entre 0.064 et 0.144. Pour des valeurs inférieures à 0.094, le modèle a prévu un maintien de la diminution du nombre de cas. Pour des valeurs égales ou supérieures à 0.104, le modèle a prévu un rebond de l’épidémie suivant le déconfinement (figure 4).</p>
<p>Le déconfinement s’est accompagné d’autres modifications - moins majeures que le taux de contact - dans les valeurs des paramètres du modèle. Par exemple le taux de sortie de l’hôpital a été réduit au cours de l’épidémie, en raison des progrès réalisés sur la prise en charge des patients.</p>
<p>En plus de la réaugmentation prévisible du taux de transmission lié à la fin du 1er confinement et à l’augmentation annuelle des déplacements pendant la période estivale, la possibilité d’une seconde vague - et donc certainement d’un second confinement - était envisagée, ne serait-ce que parce que la part de la population ayant été infectée était trop faible pour atteindre une immunité de groupe (Wise, 2020). D’autres scientifiques alertaient aussi du risque de seconde vague causé par d’autres critères, notamment l’arrivée des maladies hivernales, la fatigue du personnel médical, ou encore la réticence des gouvernements à imposer des mesures restrictives (Middleton <em>et al.</em>, 2020). En ce qui concerne la possibilité d’une troisième vague, les campagnes de vaccination ayant débuté dans plusieurs pays nous permettent d’être optimistes et d’oser penser que si elle survenait, les conséquences sur l’économie, la santé, et plus globalement notre quotidien seraient réduites.</p>
<div id="bibliographie" class="section level2">
<h2>Bibliographie</h2>
<div align="justify">
<p>Di Domenico, L., Pullano, G., Sabbatini, C. E., Boëlle, P-Y. et Colizza, C., 2020. Impact of lockdown on COVID-19 epidemic in Île-de-France and possible exit strategies. <em>BMC Medicine</em>, <strong>18</strong>:240.</p>
<p>Middleton, J., Lopes, H., Michelson, K. et Reid, J., 2020. Planning for a second wave pandemic of COVID-19 and planning for winter. <em>International Journal of Public Health</em>, <strong>65</strong>:1525–1527.</p>
<p>Roques, L., Klein, E. K., Papaïx, j., Sar, A. et Soubeyrand, S., 2020. Impact of Lockdown on the Epidemic Dynamics of COVID-19 in France. <em>Frontiers in Medicine</em>, <strong>7</strong>:274.</p>
<p>Roux, J., Massonnaud, C., et Crépey, P., 2020. COVID-19: One-month impact of the French lockdown on the epidemic burden. medRxiv.</p>
<p>Wise, J., 2020. Covid-19: Risk of second wave is very real, say researchers. <em>British Medical Journal (Online)</em>, <strong>369</strong>.</p>
<p>You, C., Deng, Y., Hu, W., Sun, J., Lin, Q., Zhou, F., … et Zhou, X. H., 2020. Estimation of the time-varying reproduction number of COVID-19 outbreak in China. <em>International Journal of Hygiene and Environmental Health</em>, <strong>228</strong>:113555.</p>
</div>
</div>




</div>

<script>

// add bootstrap table styles to pandoc tables
function bootstrapStylePandocTables() {
  $('tr.header').parent('thead').parent('table').addClass('table table-condensed');
}
$(document).ready(function () {
  bootstrapStylePandocTables();
});


</script>

<!-- tabsets -->

<script>
$(document).ready(function () {
  window.buildTabsets("TOC");
});

$(document).ready(function () {
  $('.tabset-dropdown > .nav-tabs > li').click(function () {
    $(this).parent().toggleClass('nav-tabs-open')
  });
});
</script>

<!-- code folding -->


<!-- dynamically load mathjax for compatibility with self-contained -->
<script>
  (function () {
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src  = "https://mathjax.rstudio.com/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML";
    document.getElementsByTagName("head")[0].appendChild(script);
  })();
</script>

</body>
</html>
