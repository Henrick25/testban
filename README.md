@Input() isOpenRetrait = false; // Gère l'état d'ouverture de la modale
  @Output() closeModalRetrait = new EventEmitter<void>(); // Événement pour fermer la modale
  @Output() itemSelectedRetrait = new EventEmitter<string>(); // Événement pour transmettre l'élément sélectionné

  retraits: string[] = ["Missions annexes", "Dossiers complexes", "Autres"]; // Liste des options

  // Méthode appelée lorsque l'utilisateur sélectionne un élément
  selectRetrait(item: string) {
    this.itemSelectedRetrait.emit(item); // Émet la chaîne sélectionnée
    this.closeModalRetrait.emit(); // Ferme la modale
  }

  // Méthode pour fermer explicitement la modale
  close() {
    this.closeModalRetrait.emit();
  }



<div *ngIf="isOpenRetrait" class="modal">
  <!-- Bouton pour fermer la modale -->
  <button class="btn-close-modal" (click)="close()">x</button>

  <!-- Liste des options -->
  <div 
    class="contentRetrait" 
    *ngFor="let item of retraits" 
    (click)="selectRetrait(item)">
    <button class="btn">{{ item }}</button>
  </div>
</div>


 isModalOpenRetrait = false; // État d'ouverture de la modale
  selectedItemRetrait: string | null = null; // Élément sélectionné

  // Ouvrir la modale
  openModalRetrait() {
    this.isModalOpenRetrait = true;
  }

  // Fermer la modale
  closeModalRetrait() {
    this.isModalOpenRetrait = false;
  }

  // Méthode appelée lorsque l'utilisateur sélectionne un élément
  onItemSelected(item: string) {
    this.selectedItemRetrait = item; // Stocke la chaîne sélectionnée
  }

<div>
  <!-- Affiche l'élément sélectionné -->
  <span *ngIf="selectedItemRetrait; else noSelection">
    Vous avez sélectionné : {{ selectedItemRetrait }}
  </span>
  <ng-template #noSelection>
    Aucun retrait sélectionné
  </ng-template>

  <!-- Bouton pour ouvrir la modale -->
  <button class="passer_retrait" (click)="openModalRetrait()">Ouvrir Retrait</button>

  <!-- Inclusion du composant enfant -->
  <app-retrait
    [isOpenRetrait]="isModalOpenRetrait"
    (closeModalRetrait)="closeModalRetrait()"
    (itemSelectedRetrait)="onItemSelected($event)">
  </app-retrait>
</div>
