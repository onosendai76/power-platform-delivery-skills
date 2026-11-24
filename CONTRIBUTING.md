# Cómo contribuir

Gracias por querer mejorar estas skills. Lo que más ayuda, por orden:

1. **Informes de uso.** Probaste la skill en una app: abre un issue con el tipo de app, el agente y el modelo, qué funcionó, qué no y en qué paso. Es lo que decide cuándo una skill deja de ser experimental.
2. **Correcciones concretas.** Una regla que falla, un paso que falta o un error de la plataforma que la skill no contempla. Mejor con un caso reproducible.
3. **Skills nuevas.** Abre antes un issue para hablarlo, para no duplicar lo que ya hacen las skills oficiales de Microsoft.

## Reglas

- **Nada de datos reales.** Ni secretos, ni identificadores de tenant o entorno, ni URLs privadas, ni datos de clientes en issues, pull requests o ejemplos. Usa datos ficticios.
- **Complementar, no duplicar.** Si algo ya lo cubre `power-platform-skills`, enlázalo en vez de reescribirlo.
- **Cada regla nace de una observación.** Indica de dónde sale: un laboratorio, un error real o la documentación oficial (con enlace).
- **Español primero.** La documentación principal está en español; los resúmenes en inglés son bienvenidos.
- **Commits convencionales** (`feat:`, `fix:`, `docs:`…), uno por cambio lógico.

## Versionado

Versionado semántico por skill (campo `version` del `SKILL.md`) y entrada en `CHANGELOG.md`. Mientras la versión sea 0.x, puede haber cambios incompatibles entre versiones menores.

## Licencia de las contribuciones

Al enviar una contribución aceptas que se publique bajo la licencia [MIT](LICENSE) del repositorio.
