Composant Enfant (retrait.component.ts)
Fichier TypeScript : retrait.component.ts
typescript
Copier le code
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-retrait',
  templateUrl: './retrait.component.html',
  styleUrls: ['./retrait.component.css']
})
export class RetraitComponent {
  @Input() isOpenRetrait = false;
  @Output() closeModalRetrait = new EventEmitter<void>();
  @Output() itemSelectedRetrait = new EventEmitter<any>();

  retraits = [
    { annexes: 'Missions annexes', complexes: 'Dossiers complexes', autres: 'Autres' }
    // Ajoutez d'autres objets si nécessaire
  ];

  selectRetrait(item: any) {
    this.itemSelectedRetrait.emit(item); // Émet l'élément sélectionné
    this.closeModalRetrait.emit();      // Ferme la modale
  }

  close() {
    this.closeModalRetrait.emit(); // Émet la fermeture de la modale
  }
}
Fichier HTML : retrait.component.html
html
Copier le code
<div *ngIf="isOpenRetrait" class="modal">
  <button class="btn-close-modal" (click)="close()">x</button>

  <div class="contentRetrait" *ngFor="let item of retraits" (click)="selectRetrait(item)">
    <div class="contentRetraitTop">
      <button class="annexes btn">{{ item.annexes }}</button>
      <button class="complexes btn">{{ item.complexes }}</button>
    </div>
    <div>
      <button class="btn">{{ item.autres }}</button>
    </div>
  </div>
</div>
Composant Parent (parent.component.ts)
Fichier TypeScript : parent.component.ts
typescript
Copier le code
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  templateUrl: './parent.component.html',
  styleUrls: ['./parent.component.css']
})
export class ParentComponent {
  isModalOpenRetrait = false;
  selectedItemRetrait: string | null = null;
  isLoggedIn = true; // Simule un utilisateur connecté

  openModalRetrait() {
    this.isModalOpenRetrait = true; // Ouvre la modale
  }

  closeModalRetrait() {
    this.isModalOpenRetrait = false; // Ferme la modale
  }

  onItemSelected(item: any) {
    if (item) {
      this.selectedItemRetrait = item.annexes || item.complexes || item.autres; // Définit la valeur de l'élément sélectionné
    }
  }
}
Fichier HTML : parent.component.html
html
Copier le code
<span *ngIf="isLoggedIn">
  {{ selectedItemRetrait || 'Retrait' }}
  <button class="passer_retrait" (click)="openModalRetrait()">Ouvrir Retrait</button>
</span>

<app-retrait
  [isOpenRetrait]="isModalOpenRetrait"
  (closeModalRetrait)="closeModalRetrait()"
  (itemSelectedRetrait)="onItemSelected($event)">
</app-retrait>
Structure des fichiers
La structure des fichiers serait organisée comme suit :

css
Copier le code
src/
├── app/
│   ├── parent/
│   │   ├── parent.component.ts
│   │   ├── parent.component.html
│   │   ├── parent.component.css
│   ├── retrait/
│   │   ├── retrait.component.ts
│   │   ├── retrait.component.html
│   │   ├── retrait.component.css
Explication des ajustements :
Templates HTML séparés :
Le code HTML des composants parent et enfant est dans des fichiers .html distincts.

Événements émis correctement :

L'enfant émet l'élément sélectionné via (itemSelectedRetrait).
L'enfant émet également un événement pour fermer la modale via (closeModalRetrait).
Mise à jour dans le parent :

Le parent met à jour selectedItemRetrait avec la valeur de l'élément sélectionné.
Résultat attendu
Le composant parent affiche "Retrait" par défaut.
Lorsqu'un utilisateur clique sur "Ouvrir Retrait", la modale apparaît.
En sélectionnant un élément dans la modale, cet élément est transmis au parent et affiché à la place de "Retrait". La modale se ferme automatiquement.





Vous avez dit :
d'ou venait mon erreur ?
ChatGPT a dit :
ChatGPT
Votre erreur vient probablement de la gestion de la sélection de l'élément dans le composant enfant et de son passage au composant parent. Voici quelques points spécifiques où les erreurs peuvent se produire :

1. Problème dans la méthode selectRetrait (composant enfant) :
La méthode selectRetrait dans le composant enfant utilise cette ligne :

typescript
Copier le code
this.itemSelectedRetrait.emit(item);
Cela fonctionne bien, mais si cette ligne n'était pas correctement appelée, ou si elle émettait une mauvaise valeur (par exemple, si item n'était pas transmis correctement), cela pourrait entraîner le comportement où seul le premier élément de la liste est sélectionné.

2. Gestion incorrecte de l'événement dans le parent :
Dans le parent, la méthode onItemSelected devrait mettre à jour correctement selectedItemRetrait. Voici votre code initial :

typescript
Copier le code
updateSelectedItemValue() {
    console.log('Valeur actuelle de selectedItem:', this.selectedItemRetrait.id); 
    if (this.selectedItemRetrait) {
        this.selectedItemRetrait = this.selectedItemRetrait.annexes || this.selectedItemRetrait.complexes || this.selectedItemRetrait.autres;
    } else {
        this.selectedItemRetrait = null;
    }
}
Problèmes potentiels :

Vous appelez updateSelectedItemValue au lieu de simplement traiter onItemSelected dans la gestion de l'événement.
La condition this.selectedItemRetrait.id pourrait provoquer des erreurs si this.selectedItemRetrait n'a pas de propriété id.
Si selectedItemRetrait n'est pas correctement mis à jour avec la valeur émise, il reste inchangé.
3. Problème dans le template HTML du parent :
Votre initialisation dans le parent pourrait ne pas réagir correctement à selectedItemRetrait. Par exemple, dans ce code :

html
Copier le code
{{ selectedItemRetrait || 'Retrait' }}
Si selectedItemRetrait est mal mis à jour (par exemple, s'il reste null), il affichera toujours "Retrait". Cela donnerait l'impression que seul le premier élément est sélectionné.

4. Le binding de l'événement n'était pas correct :
Assurez-vous que dans le parent, l'événement (itemSelectedRetrait) est bien lié au bon gestionnaire :

html
Copier le code
<app-retrait 
  [isOpenRetrait]="isModalOpenRetrait"
  (closeModalRetrait)="closeModalRetrait()"
  (itemSelectedRetrait)="onItemSelected($event)">
</app-retrait>
