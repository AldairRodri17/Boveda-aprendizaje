---
proyecto: Conocimiento
tipo: laboratorio
nivel: intermedio
progreso: pendiente
estado: borrador
area: Angular
herramienta: Angular CLI
tags: [angular, laboratorio]
---
# Lab 4 - Formularios reactivos con validación en Angular

Parte de: [[20-Angular/02 Intermedio/_Índice Intermedio|Intermedio]]

**Cubre:** [[Formularios reactivos]] · [[Validaciones personalizadas]] · [[Formularios template-driven]]

> [!info] Al terminar
> Si todos los criterios quedan marcados, cambia a `dominado` el `progreso` de este laboratorio y de las notas que cubre. Guarda tus archivos de práctica en esta misma carpeta.

## Objetivo

Construir un formulario de registro con formularios reactivos, validadores integrados, validadores personalizados (de campo y de grupo) y mensajes de error claros.

## Reglas del formulario

| Campo | Reglas |
|---|---|
| Nombre | requerido, mínimo 3 caracteres |
| Correo | requerido, formato de correo |
| Contraseña | requerida, mínimo 8 caracteres, al menos un número |
| Confirmar contraseña | igual a la contraseña |
| Edad | opcional; si se escribe, mínimo 18 |
| Acepto términos | obligatorio marcarlo |

## Pasos

1. Crea un componente `registro` que importe `ReactiveFormsModule`.
2. Crea el formulario con `NonNullableFormBuilder` según la tabla.
3. Escribe en `validadores.ts` los validadores `contieneNumero` (campo) y `passwordsIguales` (grupo).
4. En la plantilla, muestra los errores de cada campo **solo después** de que el usuario lo toque.
5. Deshabilita **Registrar** mientras el formulario sea inválido; al enviar, muestra en pantalla el JSON con `getRawValue()` (sin la confirmación) y reinicia el formulario.
6. Muestra en vivo el estado del formulario (`VALID`, `INVALID`) para depurar.
7. **Reto:** agrega un `FormArray` de teléfonos (botones *Agregar teléfono* y *Quitar*), cada uno de 10 dígitos, y un validador asíncrono que marque como ocupado el correo `admin@demo.com` tras 500 ms.

## Criterios de terminado

- [ ] Al cargar no se ve ningún error; al salir de un campo inválido aparece su mensaje.
- [ ] `ana@` muestra "Correo no válido"; `abcdefgh` en contraseña muestra "Debe incluir un número".
- [ ] Contraseñas distintas muestran "Las contraseñas no coinciden" (error del grupo).
- [ ] Edad 16 muestra error; edad vacía es válida.
- [ ] El botón Registrar solo se habilita cuando todo es válido y los términos están aceptados.
- [ ] Al enviar se muestra el JSON y el formulario vuelve a su estado inicial.
- [ ] Puedo explicar qué cambiaría si lo hiciera template-driven.
- [ ] Reto: los teléfonos se agregan y quitan, y `admin@demo.com` muestra "Correo ya registrado".

## Solución

> [!tip] Los `import` de Angular y RxJS se omiten en algunos fragmentos para abreviar; VS Code los agrega con **Ctrl + .** sobre el nombre marcado en rojo.

> [!success]- validadores.ts
> ```ts
> import { AbstractControl, ValidationErrors, ValidatorFn } from '@angular/forms';
>
> export const contieneNumero: ValidatorFn = (c: AbstractControl): ValidationErrors | null =>
>   /\d/.test(c.value ?? '') ? null : { sinNumero: true };
>
> export const passwordsIguales: ValidatorFn = (g: AbstractControl): ValidationErrors | null =>
>   g.get('password')?.value === g.get('confirmar')?.value ? null : { noCoinciden: true };
> ```

> [!success]- registro.ts
> ```ts
> @Component({
>   selector: 'app-registro',
>   imports: [ReactiveFormsModule, JsonPipe],
>   templateUrl: './registro.html',
> })
> export class Registro {
>   private fb = inject(NonNullableFormBuilder);
>   enviado: unknown = null;
>
>   form = this.fb.group({
>     nombre: ['', [Validators.required, Validators.minLength(3)]],
>     correo: ['', [Validators.required, Validators.email]],
>     password: ['', [Validators.required, Validators.minLength(8), contieneNumero]],
>     confirmar: ['', Validators.required],
>     edad: this.fb.control<number | null>(null, Validators.min(18)),
>     terminos: [false, Validators.requiredTrue],
>   }, { validators: passwordsIguales });
>
>   get c() { return this.form.controls; }
>
>   enviar() {
>     if (this.form.invalid) { this.form.markAllAsTouched(); return; }
>     const { confirmar, ...datos } = this.form.getRawValue();
>     this.enviado = datos;
>     this.form.reset();
>   }
> }
> ```
> `Validators.min` no marca error cuando el valor es `null`, por eso la edad vacía es válida.

> [!success]- registro.html (extracto)
> ```html
> <form [formGroup]="form" (ngSubmit)="enviar()">
>   <label>Nombre <input formControlName="nombre" /></label>
>   @if (c.nombre.touched && c.nombre.hasError('required')) { <small>El nombre es obligatorio</small> }
>   @if (c.nombre.touched && c.nombre.hasError('minlength')) { <small>Mínimo 3 caracteres</small> }
>
>   <label>Correo <input formControlName="correo" type="email" /></label>
>   @if (c.correo.touched && c.correo.hasError('email')) { <small>Correo no válido</small> }
>
>   <label>Contraseña <input formControlName="password" type="password" /></label>
>   @if (c.password.touched && c.password.hasError('minlength')) { <small>Mínimo 8 caracteres</small> }
>   @if (c.password.touched && c.password.hasError('sinNumero')) { <small>Debe incluir un número</small> }
>
>   <label>Confirmar <input formControlName="confirmar" type="password" /></label>
>   @if (c.confirmar.touched && form.hasError('noCoinciden')) { <small>Las contraseñas no coinciden</small> }
>
>   <label>Edad <input formControlName="edad" type="number" /></label>
>   @if (c.edad.hasError('min')) { <small>Debes tener al menos 18</small> }
>
>   <label><input formControlName="terminos" type="checkbox" /> Acepto términos</label>
>
>   <button [disabled]="form.invalid">Registrar</button>
>   <p>Estado: {{ form.status }}</p>
> </form>
> @if (enviado) { <pre>{{ enviado | json }}</pre> }
> ```

> [!success]- Template-driven frente a reactivo
> En template-driven usarías `FormsModule`, `[(ngModel)]` con `name`, atributos `required minlength="3" email`, y para la regla de "contiene número" o "contraseñas iguales" tendrías que escribir **directivas** validadoras. El modelo se construye desde la plantilla y es más difícil de probar.

> [!success]- Reto: FormArray y validador asíncrono
> ```ts
> telefonos = this.fb.array<FormControl<string>>([]);   // agrégalo al group como telefonos: this.telefonos
> agregarTel() { this.telefonos.push(this.fb.control('', [Validators.required, Validators.pattern(/^\d{10}$/)])); }
> quitarTel(i: number) { this.telefonos.removeAt(i); }
>
> correoLibre: AsyncValidatorFn = c =>
>   timer(500).pipe(map(() => (c.value === 'admin@demo.com' ? { ocupado: true } : null)));
> // correo: ['', { validators: [Validators.required, Validators.email], asyncValidators: [this.correoLibre], updateOn: 'blur' }]
> ```
> ```html
> <div formArrayName="telefonos">
>   @for (t of telefonos.controls; track $index; let i = $index) {
>     <input [formControlName]="i" /> <button type="button" (click)="quitarTel(i)">Quitar</button>
>   }
> </div>
> <button type="button" (click)="agregarTel()">Agregar teléfono</button>
> ```
> Declara `correoLibre` **antes** de `form` en la clase para que exista cuando se crea el formulario.

## Notas personales

_Qué me costó, errores que cometí, dudas._
