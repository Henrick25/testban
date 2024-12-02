# testban

<div class="modal" *ngIf="isOpenRetrait">
    <div class="modal-content">
        <div class="titleRetrait">
            <p> Veuillez selectionner votre retrait

            </p>
            <button class="btn-close-modal" (click)="close()">x</button>
        </div>
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
</div>
export class RetraitComponent {
    @Input() isOpenRetrait = false;
    @Output() closeModalRetrait = new EventEmitter<void>();
    @Output() itemSelectedRetrait = new EventEmitter<any>();


    selectedRetrait: any = null;
    retraits = [
        {annexes: 'Missions annexes', complexes: 'Dossiers complexes', autres: 'Autres'}
        // Ajoute d'autres objets si nécessaire
    ];

    selectRetrait(item: any) {
        this.selectedRetrait = item;
        this.itemSelectedRetrait.emit(item);
        this.closeModalRetrait.emit();
    }

    close() {
        this.closeModalRetrait.emit();
    }
    //parent
isModalOpenRetrait = false;


    openModalRetrait() {
        this.isModalOpenRetrait = true;
    }

    closeModalRetrait() {
        this.isModalOpenRetrait = false;
    }

    onItemSelected(item: any) {
        this.selectedItemRetrait = item;
        this.updateSelectedItemValue();
    }

    updateSelectedItemValue() {
        console.log('Valeur actuelle de selectedItem:', this.selectedItemRetrait.id); // Log la valeur actuelle de selectedItem
        if (this.selectedItemRetrait) {
            this.selectedItemRetrait = this.selectedItemRetrait.annexes || this.selectedItemRetrait.complexes || this.selectedItemRetrait.autres;
        } else {
            this.selectedItemRetrait = null;
        }
    }
<span *ngIf="isLoggedIn">{{ selectedItemRetrait || 'Retrait' }}</span>
  <button class="passer_retrait" (click)="openModalRetrait()"></button>
                                    <app-retrait
                                        [isOpenRetrait]="isModalOpenRetrait"
                                        (closeModalRetrait)="closeModalRetrait()"
                                        (itemSelectedRetrait)="onItemSelected($event)">
                                    </app-retrait>
