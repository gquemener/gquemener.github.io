---
layout: post
title: "Idées reçues des programmeurs sur les noms"
date: 2025-08-28 20:00:00 +01:00
categories: database
---

_Reading time: 6-7 minutes_

_Cet article est une traduction d’[un article](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) écrit par Patrick McKenzie, le 17/06/2010. Je l’ai trouvé inspirant, et il reflète un certain nombres de situations que j’ai moi-même vécues à titre personnel ou que j’ai pues observer dans ma vie professionnelle. C’est ce qui m’a motivé à le traduire, afin d’en faire profiter un maximum de francophone!_

John Graham-Cumming a écrit [un article](http://blog.jgc.org/2010/06/your-last-name-contains-invalid.html) en se plaignant d’un formulaire sur un site internet qui refusait son nom de famille, au prétexte qu’il contenait des caractères invalides. Ce n’était évidemment pas le cas, car peu importe ce qu’une personne affirme être son nom, cela est - par définition - une façon appropriée de l’identifier. John était, à raison, vexé par cette situation car le nom est **une information centrale pour notre identité**.

J’ai vécu au Japon pendant quelques années, exerçant le métier de programmeur, et j’ai cassé un certain nombre de systèmes simplement en tentant de présenter mon identité. La plupart des gens m’appelle Patrick McKenzie, mais je considère correcte n’importe laquelle des six façons de me nommer. Aucun des systèmes avec lesquelles j’ai été en contact n’acceptait précisément une de celles-ci. De la même façon, j’ai travaillé avec Big Freaking Enterprise qui, à force de travailler à l’international, avait théoriquement designé leurs systèmes pour accepter n’importe quel nom. **Je n’ai jamais vu un système informatique géré les noms correctement, et doute qu’il en existe un, où qu’il soit.**

Ainsi, afin de nous rendre service à tous, je vais lister des idées reçues que votre système fait à propos des noms. **Toutes ces idées reçues sont fausses.** Essayer d’en faire moins la prochaine fois que vous écrirez un système qui touche à des noms.

1. Les gens ont exactement un nom unique.
2. Les gens ont exactement une façon d’être appelés.
3. Les gens ont exactement, à ce moment précis, un nom unique.
4. Les gens ont exactement, à ce moment précis, une façon d’être appelés.
5. Les gens ont exactement N noms, pour n’importe quelle valeur de N.
6. Le nom des gens tient dans un espace prédéterminé.
7. Le nom des gens ne change pas.
8. Le nom des gens change, mais seulement à certains moments prédéterminés.
9. Le nom des gens ne contient que des caractères ASCII.
10. Le nom des gens est écrit dans un unique ensemble de caractères.
11. Le nom des gens ne contient que des caractères associables à des _code points_ Unicode.
12. Le nom des gens est sensible à la casse.
13. Le nom des gens est insensible à la casse.
14. Le nom des gens contient des préfixes ou des suffixes, mais vous pouvez les ignorer sans problème.
15. Le nom des gens ne contient pas de chiffres.
16. Le nom des gens n’est pas écrit en MAJUSCULES.
17. Le nom des gens n’est pas écrit entièrement en minuscules.
18. Les noms des gens sont ordonnés. Sélectionner un schéma d’ordonnancement résultera automatiquement dans un ordre cohérent parmis tous les systèmes, tant que ces systèmes utilisent le même schéma d’ordonnancement.
19. Le prénom et le nom de famille des gens sont nécessairement différents.
20. Les gens ont un nom de famille, où quelque chose qu’ils partagent avec leur famille afin de les reconnaître comme ayant un lien de parenté.
21. Le nom des gens est mondialement unique.
22. Le nom des gens est _presque_ mondialement unique.
23. D’accord, d’accord, mais les noms des gens sont assez diversifiés pour qu’il n’y ait pas des millions de personnes qui partagent le même nom.
24. Mon système n’aura jamais à traîter des noms en provenance de Chine.
25. ou du Japon.
26. ou de Corée.
27. ou d’Irlande, du Royaume Uni, des États-Unis, d’Espagne, du Mexique, du Brésil, du Pérou, de Russie, de Suède, du Botswana, d’Afrique du Sud, de Trinidad, d’Haïti, de France, ou de l’empire Klingon, qui ont tous des noms “bizarres” de façon habituelle.
28. Le truc avec le Klingon était une blague, hein ?
29. Au diable votre diversité culturelle ! Chez moi, les gens se sont accordés pour avoir une façon standard de se nommer !
30. Il existe un algorithme qui transforme des noms et qui peut être inversé sans aucune perte. (Oui, oui, vous pouvez le faire si votre algorithme retourne son entrée, bravo voilà une médaille.)
31. Je peux supposer que cette liste ne contient aucun nom.
32. Le nom des gens est assigné à la naissance.
33. Ok, peut-être pas à la naissance, mais assez proche de la naissance.
34. D’accord, d’accord, dans la première année à peu prés suivant la naissance.
35. Cinq ans ?
36. Vous vous moquez de moi, hein ?
37. Deux systèmes contenant des données sur une personne utiliseront le même nom pour cette personne.
38. Deux opérateurs en charge de renseigner le nom d’une personne dans un système, entreront exactement la même séquence de bits sous forme de chaîne de caractères sur n’importe quel système, si celui-ci est bien designé.
39. Les gens dont le nom casse mon système sont des cas à la marge. Ils devraient avoir un nom solide et acceptable, comme 田中太郎.
40. Les gens ont un nom.

Cette liste n’est absolument pas exhaustive. Si vous avez besoin d’exemples qui réfutent n’importe laquelle de ces idées reçues, je vous en fournirais avec plaisir. N’hésitez pas à ajouter d’autres idées reçues en commentaires, et à partagez cet article la prochaine fois qu’un collègue suggére l’idée de génie d’ajouter des colonnes first_name et last_name.
