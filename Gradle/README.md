依赖分析：`gradle dependencies`

只分析compile环境：`gradle dependencies -q --configuration compile`

找出slf4j-api被哪些包依赖：`gradle dependencyInsight --dependency slf4j-api`

