
```swift
   func setTableView() {
        someObservable.bind(to: phoneBookView.tableView.rx.items(cellIdentifier: TableView.cellIdent, cellType: PhoneBookTableViewCell.self)) 
        { (row, item, cell) in
        	//구현 부분
        }.disposed(by: disposeBag)
    }
    // 셀 클릭
    func tableViewCellSelected() {
        tableView.rx.itemSelected
            .subscribe(onNext: { indexPath in
			//구현부분
            }).disposed(by: disposeBag)
    }
```

Tip: get item in indexPath.row
```swift
someObservable.compactMap { array in
          array.indices.contains(indexPath.row) ? array[indexPath.row] : nil
        }
```