
---
Task ID: acf-fields-update
Agent: main (Z.ai Code)
Task: External repo /home/z/ns_manager_app (github.com/jallerniki/Ns_manager_app): add ACF fields to product editor — "комментарий_" textarea, "чья_ткань_" accordion+radio with visible selection, composition builder for "состав_" ([число]% [материал] rows, legacy text storage format).

Work Log:
- Cloned user's repo to /home/z/ns_manager_app; studied worklog, wp-types.ts, api.ts, product-editor.tsx.
- Implemented 3 features (2 new components + editor integration), verified end-to-end with local mock WP API (port 3101) via agent-browser (login → editor → parse/serialize composition → accordion radio → save → PUT payload correct), mobile+desktop screenshots clean.
- Production static export build verified: out/index.html, basePath /manager_app/ intact.

Stage Summary:
- "состав_" storage format unchanged (plain text "95% хлопок, 5% вискоза"), parser handles "шёлк"/"100% шёлк"/multi-part formats.
- CHYA_TKAN_OPTIONS constant in wp-types.ts must be adjusted by the user to match their ACF choices.
- Full details in /home/z/ns_manager_app/worklog.md (Task ID: acf-fields-update).

---
Task ID: composition-attribute-source
Agent: main (Z.ai Code)
Task: Ns_manager_app — список материалов конструктора состава теперь берётся из глобального атрибута WooCommerce "составы для менеджера" (sostavy-dlya-menedzhera) через REST.

Work Log:
- /home/z/ns_manager_app: api.ts (listAttributes/getAttributeTerms/fetchCompositionMaterials с localStorage-кэшем и фолбэком), wp-types.ts (COMPOSITION_ATTRIBUTE_SLUG + типы), composition-builder.tsx (проп materials), product-editor.tsx (useQuery материалов, staleTime 10 мин).
- E2E через agent-browser + мок WP (порт 3101, эндпоинты attributes/terms с терминами пользователя): список в дропдауне = термины атрибута, поиск/выбор/сохранение работают (PUT состав_="95% хлопок, 5% шерсть вирджиния"), mobile+desktop скриншоты чистые.
- Прод-сборка пересобрана, download/manager_app2_build.zip обновлён (в /home/z/my-project/download/ и /home/z/ns_manager_app/download/).

Stage Summary:
- Материалы состава редактируются в WP-админке (атрибут "Составы для менеджера" → Configure terms), приложение подхватывает без пересборки (рефetch раз в 10 мин сессии).
- Тот же паттерн подойдёт для будущего поля "бренд".
