# Node SDK Mappers

**Project**: codemie-sdk · **Stack**: TypeScript 5.8.3, axios 1.15.0, zod 4.3.5, form-data 4.0.3 · **Scope**: response mappers and datasource type wiring in `sdk/codemie-nodejs`; service wiring is in `architecture/architecture.md`.

## Data access

### Rules
- R1. Map an API response to a client model in a static `map*` method of a `<Resource>Mapper` class in `src/mappers/` that returns `omitUndefined({...})`, so absent fields are dropped rather than set to `undefined` (biome's `noStaticOnlyClass` is off for these classes). Evidence: `sdk/codemie-nodejs/src/mappers/user.mapper.ts::UserMapper.mapAboutUserResponse`
- R2. Give a new datasource type an entry in `DATA_SOURCE_TO_PATH_PARAM`: `getKnowledgeBaseParam` calls `.toLowerCase()` on the lookup, so a type missing from it throws a `TypeError` before any request. Evidence: `sdk/codemie-nodejs/src/services/datasource.ts::DatasourceService.getKnowledgeBaseParam`
- R3. Give a new datasource type a branch in `DatasourceMapper.mapDatasourceCreateToDto`, `mapDatasourceUpdateToDto` and `mapDatasourceResponse`; the response mapper has no `XRAY` branch, so for an Xray datasource it returns only the common fields. Evidence: `sdk/codemie-nodejs/src/mappers/datasource.mapper.ts::DatasourceMapper.mapDatasourceResponse`
