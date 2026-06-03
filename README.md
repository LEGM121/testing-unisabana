# Testing Workshop - Universidad de Sabana

## Descripción del Proyecto

**Dominio**: Elegibilidad para Licencias de Conducción (`DriverLicense`)
**Objetivo**: Aplicar TDD, BDD, AAA, clases de equivalencia y cobertura de código

## Integrantes

- LEGM121 (Estudiante)

## Contenido del Wiki

Para la documentación completa del taller, consulte el **[Wiki del Repositorio](https://github.com/LEGM121/testing-unisabana/wiki)**.

### Secciones del Wiki:

1. **[Inicio](https://github.com/LEGM121/testing-unisabana/wiki)** - Dominio, alcance y equipo
2. **[TDD: Red-Green-Refactor](https://github.com/LEGM121/testing-unisabana/wiki/TDD-History)** - 3+ iteraciones
3. **[Patrón AAA](https://github.com/LEGM121/testing-unisabana/wiki/AAA-Pattern)** - Arrange-Act-Assert
4. **[Clases de Equivalencia](https://github.com/LEGM121/testing-unisabana/wiki/Equivalence-Classes)** - Tabla y justificación
5. **[BDD: Given-When-Then](BDD-Scenarios.md)** - Escenarios
6. **[Resultados](Results.md)** - JaCoCo y conclusiones
7. **[TDD History](TDD-HISTORY.md)** - Ciclos Rojo/Verde/Refactor
7. **[Defectos](https://github.com/LEGM121/testing-unisabana/wiki/Defects)** - Análisis de defectos

## Cómo Ejecutar

### Compilar y ejecutar pruebas:
```bash
mvn clean test
```

### Generar reporte de cobertura:
```bash
mvn clean test jacoco:report
```

El reporte se generará en `target/site/jacoco/index.html`

### Verificar cobertura mínima:
```bash
mvn verify
```

## Estructura del Proyecto

```
testing-unisabana/
├── src/
│   ├── main/java/
│   │   └── com/unisabana/domain/
│   │       └── DriverLicense.java     # Clase de dominio principal
│   └── test/java/
│       └── com/unisabana/domain/
│           └── DriverLicenseTest.java # Suite de pruebas
├── pom.xml                            # Configuración Maven + JaCoCo
├── .gitignore                         # Exclusiones Git
├── integrantes.txt                    # Información del equipo
└── README.md                          # Este archivo
```

## Clases de Equivalencia Cubiertas (DriverLicense)

| Clase | Rango | Tests |
|-------|-------|-------|
| TOO_YOUNG | < 16 | `shouldRejectChildrenUnder16` |
| ADOLESCENT | 16-17 | `shouldAllowRestrictedLicenseForAdolescents` |
| YOUNG_ADULT | 18-22 | `shouldAllowYoungAdults` |
| ADULT | 23-64 | `shouldAllowFullLicenseAdults` |
| SENIOR | 65-80 | `shouldAllowSeniorsWithRenewal` |
| TOO_OLD | > 80 | `shouldRejectOver80Years` |

## Valores Límite Identificados

| Límite | Valor | Test | Justificación |
|--------|-------|------|---------------|
| Mayoría de edad | 18 | `boundaryValue_AgeEighteen` | Transición minor→adult |
| Justo antes mayoría | 17 | `boundaryValue_AgeSeventeen` | Último día menor |
| Jubilación | 65 | `boundaryValue_AgeSixtyfive` | Edad legal jubilación |
| Justo antes jubilación | 64 | `boundaryValue_AgeSixtyfour` | Último año activo |
| Cambio niño→adolescente | 13 | `boundaryValue_AgeThirteen` | Inicio adolescencia |
| Último año infantil | 12 | `boundaryValue_AgeEleven` | Fin infancia |

## Patrón AAA (Arrange-Act-Assert)

Todos los tests siguen la estructura. Ejemplo aplicado a `DriverLicense`:

```java
@Test
@DisplayName("Should retrieve driver attributes correctly")
void shouldRetrieveAllAttributes() {
    // ARRANGE: Preparar datos de prueba
    DriverLicense person = new DriverLicense("1001", "Juan Pérez García", 25, false, false, 0, "REGULAR");

    // ACT: Obtener atributos
    String name = person.getFullName();

    // ASSERT: Verificar el resultado esperado
    assertThat(name).isEqualTo("Juan Pérez García");
}
```

## BDD: Escenarios Given-When-Then

Los tests siguen el estilo Given–When–Then en su descripción. Ejemplo:

```java
@Test
@DisplayName("Given a 22-year-old When applying for public service license Then should be rejected (too young)")
void shouldRejectPublicServiceUnder23() {
    // Given
    DriverLicense youngDriver = new DriverLicense("1", "Young", 22, false, false, 0, "PUBLIC_SERVICE");
    // When
    boolean isEligible = youngDriver.isEligibleForLicense();
    // Then
    assertThat(isEligible).isFalse();
}
```

## Requisitos

- Java 11+
- Maven 3.6+
- JUnit 5
- AssertJ
- JaCoCo

## Notas

- El proyecto es totalmente compilable: `mvn clean test` sin pasos adicionales
- Cobertura objetivo: ≥ 80%
- Todos los tests siguen nomenclatura: `should<Expected>When<Condition>()`
- El Wiki contiene documentación oficial (no PDF)

---

**Última actualización**: Mayo 2026  
**Estado**: En desarrollo
