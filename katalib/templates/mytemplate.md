# ${kata.title}

# Meta-Info dieser Kata

ID: ${kata.id}\
Version: ${meta.version}\
Autor: ${meta.author} [${meta.email}](mailto:${meta.email})

# Kurzbeschreibung

${kata.description}

# Ziel(e) dieser Kata

${kata.goal}

$if(kata.difficultyLevel)$
# Schwierigkeitsgrad

${kata.difficultyLevel}
$endif$

$if(kata.timeToPractice)$
# Zeitaufwand für diese Kata

Der geschätzte Zeitaufwand für diese Kata liegt bei ${kata.timeToPractice} Minuten.
$endif$

# Was du tun musst

$if(kata.introSteps)$
${kata.introSteps}
$endif$
$for(kata.steps/pairs)$
$it.key$. $it.value$
$endfor$
$if(kata.outroSteps)$
${kata.outroSteps}
$endif$

$if(kata.links)$
# Weiterführende Links

$for(kata.links)$
 - [$kata.links.title$]($kata.links.url$)$if(kata.links.comment)$: ${kata.links.comment}$endif$
$endfor$
$endif$
